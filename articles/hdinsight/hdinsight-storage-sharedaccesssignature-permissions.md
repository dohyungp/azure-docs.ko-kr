---
title: 공유 액세스 서명을 사용하여 액세스 제한 - Azure HDInsight
description: 공유 액세스 서명을 사용하여 Azure Storage Blob에 저장된 데이터에 대한 HDInsight 액세스를 제한하는 방법에 대해 알아봅니다.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive,seoapr2020
ms.date: 04/28/2020
ms.openlocfilehash: 8ab181eb72b5a3ab54ad8dba19d23288926b8969
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87006316"
---
# <a name="use-azure-storage-shared-access-signatures-to-restrict-access-to-data-in-hdinsight"></a>Azure Storage 공유 액세스 서명을 사용하여 HDInsight에서 데이터 액세스 제한

HDInsight는 클러스터와 연결된 Azure Storage 계정의 데이터에 대해 모든 액세스 권한을 갖습니다. blob 컨테이너에서 공유 액세스 서명을 사용하여 데이터에 대한 액세스를 제한할 수 있습니다. 공유 액세스 서명(SAS)은 데이터에 대한 액세스를 제한할 수 있는 Azure Storage 계정의 기능입니다. 예를 들어 데이터에 대한 읽기 전용 액세스를 제공합니다.

> [!IMPORTANT]  
> Apache Ranger를 사용하는 솔루션의 경우 도메인에 가입된 HDInsight를 사용하는 것이 좋습니다. 자세한 내용은 [도메인에 가입된 HDInsight 구성](./domain-joined/apache-domain-joined-configure.md) 문서를 참조하세요.

> [!WARNING]  
> HDInsight는 클러스터의 기본 스토리지에 대해 모든 액세스 권한이 있어야 합니다.

## <a name="prerequisites"></a>사전 요구 사항

* SSH 클라이언트. 자세한 내용은 [SSH를 사용하여 HDInsight(Apache Hadoop)에 연결](./hdinsight-hadoop-linux-use-ssh-unix.md)을 참조하세요.

* 기존 [저장소 컨테이너](../storage/blobs/storage-quickstart-blobs-portal.md)입니다.  

* PowerShell을 사용하는 경우 [Az Module](https://docs.microsoft.com/powershell/azure/)이 필요합니다.

* Azure CLI를 사용하려 하나 아직 설치하지 않은 경우 [Azure CLI 설치](https://docs.microsoft.com/cli/azure/install-azure-cli)를 참조하세요.

* [Python](https://www.python.org/downloads/)을 사용 하는 경우 버전 2.7 이상을 사용 합니다.

* C #을 사용 하는 경우 Visual Studio는 버전 2013 이상 이어야 합니다.

* 저장소 계정에 대 한 URI 체계입니다. 이 체계는 `wasb://` Azure Storage `abfs://` Azure Data Lake Storage Gen2 또는 Azure Data Lake Storage Gen1에 대 한 것입니다 `adl://` . Azure Storage에 대해 보안 전송이 활성화된 경우 URI는 `wasbs://`입니다.

* 공유 액세스 서명을 추가할 기존 HDInsight 클러스터입니다. 그렇지 않으면 Azure PowerShell을 사용하여 클러스터를 만들고 클러스터를 만들 때 공유 액세스 서명을 추가합니다.

* 의 예제 파일 [https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature) 입니다. 이 리포지토리에는 다음 항목이 포함됩니다.

  * HDInsight에 사용할 스토리지 컨테이너, 저장된 정책 및 SAS를 만들 수 있는 Visual Studio 프로젝트
  * HDInsight에 사용할 스토리지 컨테이너, 저장된 정책 및 SAS를 만들 수 있는 Python 스크립트
  * HDInsight 클러스터를 만들고 SAS를 사용하도록 구성할 수 있는 PowerShell 스크립트 업데이트 된 버전은 아래와 같이 추가로 사용 됩니다.
  * 샘플 파일:`hdinsight-dotnet-python-azure-storage-shared-access-signature-master\sampledata\sample.log`

## <a name="shared-access-signatures"></a>공유 액세스 서명

두 가지 형태의 공유 액세스 서명이 있습니다.

* `Ad hoc`: Sas의 시작 시간, 만료 시간 및 사용 권한이 SAS URI에 모두 지정 됩니다.

* `Stored access policy`: 저장 된 액세스 정책은 blob 컨테이너와 같은 리소스 컨테이너에 정의 됩니다. 정책은 하나 이상의 공유 액세스 서명에 대한 제약 조건을 관리하는 데 사용될 수 있습니다. SAS를 공유 액세스 정책과 연결할 경우 SAS는 저장된 액세스 정책에 대해 정의된 제약 조건(시작 시간, 만료 시간 및 사용 권한)을 상속합니다.

두 형식의 차이점은 주요 시나리오인 해지에 중요합니다. SAS는 URL 이므로 SAS를 획득 한 모든 사용자가 사용할 수 있습니다. 시작을 요청한 사람에 게 중요 하지 않습니다. SAS가 공개적으로 게시된 경우 전 세계의 모든 사용자가 SAS를 사용할 수 있습니다. 분산된 SAS는 다음 네 가지 중 하나에 해당할 때까지 유효합니다.

1. SAS에 지정된 만료 시간에 도달한 경우

2. SAS에서 참조된 저장된 액세스 정책에 대해 지정된 만료 시간에 도달했습니다. 다음과 같은 시나리오에서는 만료 시간에 도달합니다.

    * 시간 간격이 경과되었습니다.
    * 저장된 액세스 정책이 과거 만료 시간을 갖도록 수정되어 있습니다. SAS를 철회하는 한 가지 방법은 만료 시간을 변경하는 것입니다.

3. SAS에서 참조되는 저장된 액세스 정책을 삭제한 경우(SAS를 해지하는 다른 방법). 동일한 이름을 사용 하 여 저장 된 액세스 정책을 다시 만들면 이전 정책에 대 한 모든 SAS 토큰이 유효 합니다 (SAS의 만료 시간이 경과 하지 않은 경우). SAS를 해지하기 위해 만료 시간을 미래의 시간으로 지정하여 액세스 정책을 다시 만들 경우 다른 이름을 사용해야 합니다.

4. SAS를 만드는 데 사용된 계정 키를 다시 생성한 경우. 키를 다시 생성하면 이전 키를 사용하는 모든 애플리케이션이 인증에 실패합니다. 모든 구성 요소를 새 키로 업데이트합니다.

> [!IMPORTANT]  
> 공유 액세스 서명 URI는 서명을 만드는 데 사용된 계정 키 및 저장된 관련 액세스 정책(있는 경우)에 연결됩니다. 저장된 액세스 정책을 지정하지 않는 경우 공유 액세스 서명을 해지하는 방법은 계정 키를 변경하는 것뿐입니다.

항상 저장된 액세스 정책을 사용하는 것이 좋습니다. 저장된 정책을 사용하는 경우 필요에 따라 서명을 철회하거나 만료 날짜를 연장할 수 있습니다. 이 문서의 단계에서는 SAS를 생성하는 데 저장된 액세스 정책을 사용합니다.

공유 액세스 서명에 대한 자세한 내용은 [SAS 모델 이해](../storage/common/storage-dotnet-shared-access-signature-part-1.md)를 참조하세요.

## <a name="create-a-stored-policy-and-sas"></a>저장 된 정책 및 SAS 만들기

각 메서드 끝에 생성 된 SAS 토큰을 저장 합니다. 토큰은 다음 출력과 유사 하 게 표시 됩니다.

```output
?sv=2018-03-28&sr=c&si=myPolicyPS&sig=NAxefF%2BrR2ubjZtyUtuAvLQgt%2FJIN5aHJMj6OsDwyy4%3D
```

### <a name="using-powershell"></a>PowerShell 사용

`RESOURCEGROUP`, `STORAGEACCOUNT` 및을 `STORAGECONTAINER` 기존 저장소 컨테이너에 대 한 적절 한 값으로 바꿉니다. 디렉터리를로 변경 `hdinsight-dotnet-python-azure-storage-shared-access-signature-master` 하거나 `-File` 의 절대 경로를 포함 하도록 매개 변수를 수정 `Set-AzStorageblobcontent` 합니다. 다음 PowerShell 명령을 입력 합니다.

```powershell
$resourceGroupName = "RESOURCEGROUP"
$storageAccountName = "STORAGEACCOUNT"
$containerName = "STORAGECONTAINER"
$policy = "myPolicyPS"

# Login to your Azure subscription
$sub = Get-AzSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Connect-AzAccount
}

# If you have multiple subscriptions, set the one to use
# Select-AzSubscription -SubscriptionId "<SUBSCRIPTIONID>"

# Get the access key for the Azure Storage account
$storageAccountKey = (Get-AzStorageAccountKey `
                                -ResourceGroupName $resourceGroupName `
                                -Name $storageAccountName)[0].Value

# Create an Azure Storage context
$storageContext = New-AzStorageContext `
                                -StorageAccountName $storageAccountName `
                                -StorageAccountKey $storageAccountKey

# Create a stored access policy for the Azure storage container
New-AzStorageContainerStoredAccessPolicy `
   -Container $containerName `
   -Policy $policy `
   -Permission "rl" `
   -ExpiryTime "12/31/2025 08:00:00" `
   -Context $storageContext

# Get the stored access policy or policies for the Azure storage container
Get-AzStorageContainerStoredAccessPolicy `
    -Container $containerName `
    -Context $storageContext

# Generates an SAS token for the Azure storage container
New-AzStorageContainerSASToken `
    -Name $containerName `
    -Policy $policy `
    -Context $storageContext

<# Removes a stored access policy from the Azure storage container
Remove-AzStorageContainerStoredAccessPolicy `
    -Container $containerName `
    -Policy $policy `
    -Context $storageContext
#>

# upload a file for a later example
Set-AzStorageblobcontent `
    -File "./sampledata/sample.log" `
    -Container $containerName `
    -Blob "samplePS.log" `
    -Context $storageContext
```

### <a name="using-azure-cli"></a>Azure CLI 사용

이 섹션에서 변수를 사용 하는 것은 Windows 환경을 기반으로 합니다. Bash 또는 다른 환경에는 약간의 변형이 필요 합니다.

1. `STORAGEACCOUNT`을 `STORAGECONTAINER` 기존 저장소 컨테이너에 대 한 적절 한 값으로 바꿉니다.

    ```azurecli
    # set variables
    set AZURE_STORAGE_ACCOUNT=STORAGEACCOUNT
    set AZURE_STORAGE_CONTAINER=STORAGECONTAINER

    #Login
    az login

    # If you have multiple subscriptions, set the one to use
    # az account set --subscription SUBSCRIPTION

    # Retrieve the primary key for the storage account
    az storage account keys list --account-name %AZURE_STORAGE_ACCOUNT% --query "[0].{PrimaryKey:value}" --output table
    ```

2. 검색 된 기본 키를 나중에 사용할 수 있도록 변수로 설정 합니다. 을 `PRIMARYKEY` 이전 단계에서 검색 된 값으로 바꾸고 아래 명령을 입력 합니다.

    ```console
    #set variable for primary key
    set AZURE_STORAGE_KEY=PRIMARYKEY
    ```

3. 디렉터리를로 변경 `hdinsight-dotnet-python-azure-storage-shared-access-signature-master` 하거나 `--file` 의 절대 경로를 포함 하도록 매개 변수를 수정 `az storage blob upload` 합니다. 나머지 명령을 실행 합니다.

    ```azurecli
    # Create stored access policy on the containing object
    az storage container policy create --container-name %AZURE_STORAGE_CONTAINER% --name myPolicyCLI --account-key %AZURE_STORAGE_KEY% --account-name %AZURE_STORAGE_ACCOUNT% --expiry 2025-12-31 --permissions rl

    # List stored access policies on a containing object
    az storage container policy list --container-name %AZURE_STORAGE_CONTAINER% --account-key %AZURE_STORAGE_KEY% --account-name %AZURE_STORAGE_ACCOUNT%

    # Generate a shared access signature for the container
    az storage container generate-sas --name myPolicyCLI --account-key %AZURE_STORAGE_KEY% --account-name %AZURE_STORAGE_ACCOUNT%

    # Reversal
    # az storage container policy delete --container-name %AZURE_STORAGE_CONTAINER% --name myPolicyCLI --account-key %AZURE_STORAGE_KEY% --account-name %AZURE_STORAGE_ACCOUNT%

    # upload a file for a later example
    az storage blob upload --container-name %AZURE_STORAGE_CONTAINER% --account-key %AZURE_STORAGE_KEY% --account-name %AZURE_STORAGE_ACCOUNT% --name sampleCLI.log --file "./sampledata/sample.log"
    ```

### <a name="using-python"></a>Python 사용

파일을 열고 `SASToken.py` `storage_account_name` , 및을 `storage_account_key` `storage_container_name` 기존 저장소 컨테이너에 대 한 적절 한 값으로 바꾼 다음 스크립트를 실행 합니다.

오류 메시지가 표시 되 면를 실행 해야 할 수 있습니다 `pip install --upgrade azure-storage` `ImportError: No module named azure.storage` .

### <a name="using-c"></a>C 사용\#

1. Visual Studio에서 솔루션을 엽니다.

2. 솔루션 탐색기에서 **SASExample** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택 합니다.

3. **설정** 을 선택하고 다음 항목에 대한 값을 추가합니다.

    |항목 |Description |
    |---|---|
    |StorageConnectionString|저장된 정책 및 SAS를 만들 스토리지 계정에 대한 연결 문자열입니다. 형식은 `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey`여야 하며 여기서 `myaccount`는 사용자의 스토리지 계정 이름이고 `mykey`는 스토리지 계정에 대한 키입니다.|
    |ContainerName|액세스를 제한할 스토리지 계정의 컨테이너입니다.|
    |SASPolicyName|만들려는 저장된 정책에 사용할 이름입니다.|
    |FileToUpload|컨테이너에 업로드되는 파일의 경로입니다.|

4. 프로젝트를 실행합니다. SAS 정책 토큰, 스토리지 계정 이름 및 컨테이너 이름을 저장합니다. 스토리지 계정을 HDInsight 클러스터에 연결할 때 이러한 값이 사용됩니다.

## <a name="use-the-sas-with-hdinsight"></a>HDInsight에 SAS 사용

HDInsight 클러스터를 만들 때 기본 저장소 계정을 지정 해야 합니다. 추가 저장소 계정을 지정할 수도 있습니다. 스토리지를 추가하는 이 두 가지 방법 모두에 사용된 스토리지 계정 및 컨테이너에 대한 모든 권한이 필요합니다.

공유 액세스 서명을 사용 하 여 컨테이너 액세스를 제한 합니다. 클러스터에 대 한 **핵심 사이트** 구성에 사용자 지정 항목을 추가 합니다. PowerShell을 사용 하 여 클러스터를 만드는 동안 또는 Ambari를 사용 하 여 클러스터를 만든 후에 항목을 추가할 수 있습니다.

### <a name="create-a-cluster-that-uses-the-sas"></a>SAS를 사용하는 클러스터 만들기

`CLUSTERNAME`,, `RESOURCEGROUP` ,, 및을 `DEFAULTSTORAGEACCOUNT` `STORAGECONTAINER` `STORAGEACCOUNT` `TOKEN` 적절 한 값으로 바꿉니다. PowerShell 명령을 입력 합니다.

```powershell
$clusterName = 'CLUSTERNAME'
$resourceGroupName = 'RESOURCEGROUP'

# Replace with the Azure data center you want to the cluster to live in
$location = 'eastus'

# Replace with the name of the default storage account TO BE CREATED
$defaultStorageAccountName = 'DEFAULTSTORAGEACCOUNT'

# Replace with the name of the SAS container CREATED EARLIER
$SASContainerName = 'STORAGECONTAINER'

# Replace with the name of the SAS storage account CREATED EARLIER
$SASStorageAccountName = 'STORAGEACCOUNT'

# Replace with the SAS token generated earlier
$SASToken = 'TOKEN'

# Default cluster size (# of worker nodes), version, and type
$clusterSizeInNodes = "4"
$clusterVersion = "3.6"
$clusterType = "Hadoop"

# Login to your Azure subscription
$sub = Get-AzSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Connect-AzAccount
}

# If you have multiple subscriptions, set the one to use
# Select-AzSubscription -SubscriptionId "<SUBSCRIPTIONID>"

# Create an Azure Storage account and container
New-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $defaultStorageAccountName `
    -Location $location `
    -SkuName Standard_LRS `
    -Kind StorageV2 `
    -EnableHttpsTrafficOnly 1

$defaultStorageAccountKey = (Get-AzStorageAccountKey `
                                -ResourceGroupName $resourceGroupName `
                                -Name $defaultStorageAccountName)[0].Value

$defaultStorageContext = New-AzStorageContext `
                                -StorageAccountName $defaultStorageAccountName `
                                -StorageAccountKey $defaultStorageAccountKey

# Create a blob container. This holds the default data store for the cluster.
New-AzStorageContainer `
    -Name $clusterName `
    -Context $defaultStorageContext

# Cluster login is used to secure HTTPS services hosted on the cluster
$httpCredential = Get-Credential `
    -Message "Enter Cluster login credentials" `
    -UserName "admin"

# SSH user is used to remotely connect to the cluster using SSH clients
$sshCredential = Get-Credential `
    -Message "Enter SSH user credentials" `
    -UserName "sshuser"

# Create the configuration for the cluster
$config = New-AzHDInsightClusterConfig

$config = $config | Add-AzHDInsightConfigValue `
    -Spark2Defaults @{} `
    -Core @{"fs.azure.sas.$SASContainerName.$SASStorageAccountName.blob.core.windows.net"=$SASToken}

# Create the HDInsight cluster
New-AzHDInsightCluster `
    -Config $config `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $clusterName `
    -Location $location `
    -ClusterSizeInNodes $clusterSizeInNodes `
    -ClusterType $clusterType `
    -OSType Linux `
    -Version $clusterVersion `
    -HttpCredential $httpCredential `
    -SshCredential $sshCredential `
    -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
    -DefaultStorageAccountKey $defaultStorageAccountKey `
    -DefaultStorageContainer $clusterName

<# REVERSAL
Remove-AzHDInsightCluster `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $clusterName

Remove-AzStorageContainer `
    -Name $clusterName `
    -Context $defaultStorageContext

Remove-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $defaultStorageAccountName

Remove-AzResourceGroup `
    -Name $resourceGroupName
#>
```

> [!IMPORTANT]  
> HTTP/s 또는 SSH 사용자 이름 및 암호를 묻는 메시지가 나타나면 다음 조건을 충족하는 암호를 제공해야 합니다.
>
> * 길이가 10자 이상이어야 합니다.
> * 숫자를 1개 이상 포함해야 합니다.
> * 영숫자가 아닌 문자를 1개 이상 포함해야 합니다.
> * 대문자 또는 소문자를 1개 이상 포함해야 합니다.

이 스크립트를 완료하는 데는 일반적으로 약 15분이 소요됩니다. 스크립트가 오류 없이 완료되면 클러스터가 만들어진 것입니다.

### <a name="use-the-sas-with-an-existing-cluster"></a>기존 클러스터에서 SAS 사용

기존 클러스터가 있는 경우 다음 단계를 사용 하 여 **코어 사이트** 구성에 SAS를 추가할 수 있습니다.

1. 클러스터에 대한 Ambari 웹 UI를 엽니다. 이 페이지에 대한 주소는 `https://YOURCLUSTERNAME.azurehdinsight.net`입니다. 메시지가 표시되면 클러스터를 만들 때 사용한 관리자 이름(admin)과 암호를 사용하여 클러스터를 인증합니다.

1. **HDFS**  >  **Configs**  >  **Advanced**  >  **Custom core-site**로 이동 합니다.

1. **사용자 지정 핵심 사이트** 섹션을 확장 하 고 끝으로 스크롤한 다음 **속성 추가 ...** 를 선택 합니다. **키** 와 **값**에 다음 값을 사용 합니다.

    * **키**: `fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net`
    * **값**: 이전에 실행 된 메서드 중 하나에서 반환 된 SAS입니다.

    을 `CONTAINERNAME` c # 또는 SAS 응용 프로그램에서 사용한 컨테이너 이름으로 바꿉니다. `STORAGEACCOUNTNAME`을 사용한 저장소 계정 이름으로 바꿉니다.

    **추가** 를 선택 하 여이 키 및 값을 저장 합니다.

1. **저장** 단추를 선택 하 여 구성 변경 내용을 저장 합니다. 메시지가 표시 되 면 변경에 대 한 설명 (예: "SAS 저장소 액세스 추가")을 추가 하 고 **저장**을 선택 합니다.

    변경이 완료 되 면 **확인을** 선택 합니다.

   > [!IMPORTANT]  
   > 변경 내용을 적용하기 전에 여러 서비스를 다시 시작해야 합니다.

1. **다시 시작** 드롭다운 목록이 표시 됩니다. 드롭다운 목록에서 **영향을 받은 모두 다시 시작** 을 선택 하 고 __모두 다시 시작__을 선택 합니다.

    **MapReduce2** 및 **YARN**에 대해이 프로세스를 반복 합니다.

1. 서비스가 다시 시작된 후 각 서비스를 선택하고 **서비스 작업** 드롭다운에서 유지 관리 모드를 비활성화합니다.

## <a name="test-restricted-access"></a>제한된 액세스 테스트

SAS 저장소 계정의 항목만 읽고 나열할 수 있는지 확인 하려면 다음 단계를 사용 합니다.

1. 클러스터에 연결 합니다. 을 `CLUSTERNAME` 클러스터의 이름으로 바꾸고 다음 명령을 입력 합니다.

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

2. 컨테이너의 콘텐츠를 나열하려면 프롬프트에서 다음 명령을 사용합니다.

    ```bash
    hdfs dfs -ls wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
    ```

    `SASCONTAINER`을 SAS 저장소 계정에 대해 만든 컨테이너의 이름으로 바꿉니다. `SASACCOUNTNAME`을 SAS에 사용 되는 저장소 계정 이름으로 바꿉니다.

    목록에는 컨테이너와 SAS를 만들 때 업로드된 파일이 포함됩니다.

3. 다음 명령을 사용하여 파일의 내용을 읽을 수 있는지 확인합니다. `SASCONTAINER` `SASACCOUNTNAME` 이전 단계에서와 같이를 바꿉니다. `sample.log`을 이전 명령에 표시 되는 파일의 이름으로 바꿉니다.

    ```bash
    hdfs dfs -text wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/sample.log
    ```

    이 명령은 파일 내용을 나열합니다.

4. 다음 명령을 사용하여 파일을 로컬 파일 시스템에 다운로드합니다.

    ```bash
    hdfs dfs -get wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/sample.log testfile.txt
    ```

    이 명령은 파일을 **testfile.txt**라는 로컬 파일에 다운로드합니다.

5. 다음 명령을 사용하여 로컬 파일을 SAS 스토리지의 새 **testupload.txt** 파일에 업로드합니다.

    ```bash
    hdfs dfs -put testfile.txt wasbs://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
    ```

    다음 텍스트와 유사한 메시지가 표시됩니다.

    ```output
    put: java.io.IOException
    ```

    스토리지 위치가 읽기 + 목록 전용이므로 이 오류가 발생합니다. 다음 명령을 사용하여 쓰기 가능한 클러스터의 기본 스토리지에 데이터를 저장합니다.

    ```bash
    hdfs dfs -put testfile.txt wasbs:///testupload.txt
    ```

    이때 작업이 성공적으로 완료되어야 합니다.

## <a name="next-steps"></a>다음 단계

HDInsight 클러스터에 제한 된 액세스 저장소를 추가 하는 방법을 배웠으므로 이제 클러스터에서 데이터로 작업 하는 다른 방법에 대해 알아보세요.

* [HDInsight와 함께 SSH 사용](hdinsight-hadoop-linux-use-ssh-unix.md)
* [사용자에게 Apache Ambari Views에 대한 권한 부여](hdinsight-authorize-users-to-ambari.md)

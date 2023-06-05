# Azure API 분석 (WIP)

## References

- [https://learn.microsoft.com/en-us/azure/cognitive-services/computer-vision/overview-ocr](https://learn.microsoft.com/en-us/azure/cognitive-services/computer-vision/overview-ocr)
- [https://learn.microsoft.com/en-us/azure/cognitive-services/computer-vision/quickstarts-sdk/client-library?tabs=visual-studio&pivots=programming-language-csharp](https://learn.microsoft.com/en-us/azure/cognitive-services/computer-vision/quickstarts-sdk/client-library?tabs=visual-studio&pivots=programming-language-csharp)
- [https://learn.microsoft.com/en-us/azure/cognitive-services/computer-vision/how-to/call-read-api](https://learn.microsoft.com/en-us/azure/cognitive-services/computer-vision/how-to/call-read-api)
- [https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2/operations/5d986960601faab4bf452005](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2/operations/5d986960601faab4bf452005)

## Azure OCR 개요

- OCR 엔진 
Read는 배포 flexibility를 위해 Cloud service 및 on-premises container로 사용할 수 있음.
- OCR Read Version

| Input | Examples | Read edition | Benefit |
| --- | --- | --- | --- |
| Images: General, in-the-wild images | labels, street signs, and posters | https://learn.microsoft.com/en-us/azure/cognitive-services/computer-vision/concept-ocr | Optimized for general, non-document images with a performance-enhanced synchronous API that makes it easier to embed OCR in your user experience scenarios. |
| Documents: Digital and scanned, including images | books, articles, and reports | https://learn.microsoft.com/en-us/azure/applied-ai-services/form-recognizer/concept-read | Optimized for text-heavy scanned and digital documents with an asynchronous API to help automate intelligent document processing at scale. |
- OCR 클라우드 API 사용 또는 on-premises container 배포 
클라우드 API : 통합이 쉽고 즉시 사용 가능한 빠른 생산성 때문에 대부분의 고객이 선호하는 옵션 
on-premises container : Read Docker 컨테이너를 사용하면 Computer Vision v3.2 일반적으로 사용 가능한 OCR 기능을 자체 로컬 환경에 배포할 수 있음. 컨테이너는 특정 보안 및 데이터 거버넌스 요구 사항에 적합함.

## Computer Vision v3.2 GA Read

- 전제 조건 
1. Azure 구독 
2. Visual Studio IDE 또는 현재 버전의 .NET Core 
3. Azure 구독이 있으면 Azure Portal에서 Computer Vision 리소스를 만들어 키와 엔드포인트를 가져옵니다. 배포 후 리소스로 이동을 클릭
- **인쇄 및 필기 텍스트 읽기**
1. 새 C# 애플리케이션을 만듭니다.
    - [비주얼 스튜디오 IDE](https://learn.microsoft.com/en-us/azure/cognitive-services/computer-vision/quickstarts-sdk/client-library?tabs=visual-studio&pivots=programming-language-csharp#tabpanel_1_visual-studio)
    - [CLI](https://learn.microsoft.com/en-us/azure/cognitive-services/computer-vision/quickstarts-sdk/client-library?tabs=visual-studio&pivots=programming-language-csharp#tabpanel_1_cli)
    
    Visual Studio를 사용하여 새 .NET Core 애플리케이션을 만듭니다.
    
    **클라이언트 라이브러리 설치**
    
    **새 프로젝트를 만들었으면 솔루션 탐색기** 에서 프로젝트 솔루션을 마우스 오른쪽 단추로 클릭 하고 **NuGet 패키지 관리를** 선택하여 클라이언트 라이브러리를 설치합니다 . 열리는 패키지 관리자에서 **Browse 를 선택하고 Include prerelease 를** 선택한 다음 를 검색하십시오 `Microsoft.Azure.CognitiveServices.Vision.ComputerVision`. 버전을 선택한 `7.0.0`다음 **설치를** 선택합니다 .
    
2. 키와 끝점을 찾습니다.
    
    Azure Portal로 이동합니다. **사전 요구 사항** 섹션 에서 생성한 Computer Vision 리소스가 성공적으로 배포되었으면 **다음 단계** 아래 **리소스로 이동** 버튼을 클릭합니다 . **리소스 관리** 아래 리소스의 **키 및 엔드포인트** 페이지 에서 키와 엔드포인트를 찾을 수 있습니다 . 리소스 키는 Azure 구독 ID와 동일하지 않습니다.
    
3. 프로젝트 디렉터리에서 선호하는 편집기 또는 IDE에서 *Program.cs 파일을 엽니다. Program.cs* 의 내용을 다음 코드로 바꿉니다.
    
    C# 
    
    ```csharp
    using System;
    using System.Collections.Generic;
    using Microsoft.Azure.CognitiveServices.Vision.ComputerVision;
    using Microsoft.Azure.CognitiveServices.Vision.ComputerVision.Models;
    using System.Threading.Tasks;
    using System.IO;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    using System.Threading;
    using System.Linq;
    
    namespace ComputerVisionQuickstart
    {
        class Program
        {
            // Add your Computer Vision subscription key and endpoint
            static string subscriptionKey = "PASTE_YOUR_COMPUTER_VISION_SUBSCRIPTION_KEY_HERE";
            static string endpoint = "PASTE_YOUR_COMPUTER_VISION_ENDPOINT_HERE";
    
            private const string READ_TEXT_URL_IMAGE = "https://raw.githubusercontent.com/Azure-Samples/cognitive-services-sample-data-files/master/ComputerVision/Images/printed_text.jpg";
    
            static void Main(string[] args)
            {
                Console.WriteLine("Azure Cognitive Services Computer Vision - .NET quickstart example");
                Console.WriteLine();
    
                ComputerVisionClient client = Authenticate(endpoint, subscriptionKey);
    
                // Extract text (OCR) from a URL image using the Read API
                ReadFileUrl(client, READ_TEXT_URL_IMAGE).Wait();
            }
    
            public static ComputerVisionClient Authenticate(string endpoint, string key)
            {
                ComputerVisionClient client =
                  new ComputerVisionClient(new ApiKeyServiceClientCredentials(key))
                  { Endpoint = endpoint };
                return client;
            }
    
            public static async Task ReadFileUrl(ComputerVisionClient client, string urlFile)
            {
                Console.WriteLine("----------------------------------------------------------");
                Console.WriteLine("READ FILE FROM URL");
                Console.WriteLine();
    
                // Read text from URL
                var textHeaders = await client.ReadAsync(urlFile);
                // After the request, get the operation location (operation ID)
                string operationLocation = textHeaders.OperationLocation;
                Thread.Sleep(2000);
    
                // Retrieve the URI where the extracted text will be stored from the Operation-Location header.
                // We only need the ID and not the full URL
                const int numberOfCharsInOperationId = 36;
                string operationId = operationLocation.Substring(operationLocation.Length - numberOfCharsInOperationId);
    
                // Extract the text
                ReadOperationResult results;
                Console.WriteLine($"Extracting text from URL file {Path.GetFileName(urlFile)}...");
                Console.WriteLine();
                do
                {
                    results = await client.GetReadResultAsync(Guid.Parse(operationId));
                }
                while ((results.Status == OperationStatusCodes.Running ||
                    results.Status == OperationStatusCodes.NotStarted));
    
                // Display the found text.
                Console.WriteLine();
                var textUrlFileResults = results.AnalyzeResult.ReadResults;
                foreach (ReadResult page in textUrlFileResults)
                {
                    foreach (Line line in page.Lines)
                    {
                        Console.WriteLine(line.Text);
                    }
                }
                Console.WriteLine();
            }
    
        }
    }
    
    ```
    
4. 표시된 코드에 키와 엔드포인트를 붙여넣습니다. Computer Vision 엔드포인트의 형식은 입니다 `https://<your_computer_vision_resource_name>.cognitiveservices.azure.com/`.
    
    **중요한**
    
    완료되면 코드에서 키를 제거하고 공개적으로 게시하지 마십시오. **[프로덕션의 경우 Azure Key Vault](https://learn.microsoft.com/en-us/azure/key-vault/general/overview)** 와 같은 자격 증명을 안전하게 저장하고 액세스하는 방법을 사용하세요 . 자세한 내용은 Cognitive Services **[보안 문서를 참조하세요.](https://learn.microsoft.com/en-us/azure/cognitive-services/cognitive-services-security)**
    
5. 선택적 단계로 [모델 버전 지정 방법을](https://learn.microsoft.com/en-us/azure/cognitive-services/computer-vision/how-to/call-read-api#determine-how-to-process-the-data-optional) 참조하세요 . 예를 들어 최신 GA 모델을 명시적으로 지정하려면 `ReadAsync`다음과 같이 호출을 편집합니다. 매개변수를 건너뛰거나 `"latest"`자동으로 사용하면 최신 GA 모델이 사용됩니다.
    
    씨#복사
    
    ```
      // Read text from URL with a specific model version
      var textHeaders = await client.ReadAsync(urlFile,null,null,"2022-04-30");
    
    ```
    
6. 응용 프로그램을 실행하십시오.
    - [비주얼 스튜디오 IDE](https://learn.microsoft.com/en-us/azure/cognitive-services/computer-vision/quickstarts-sdk/client-library?tabs=visual-studio&pivots=programming-language-csharp#tabpanel_2_visual-studio)
    - [CLI](https://learn.microsoft.com/en-us/azure/cognitive-services/computer-vision/quickstarts-sdk/client-library?tabs=visual-studio&pivots=programming-language-csharp#tabpanel_2_cli)
    
    IDE 창 상단에 있는 **디버그** 버튼을 클릭합니다 .
    

## Call the Computer Vision 3.2 GA Read API

- 입력 요구 사항 
지원되는 파일 형식: JPEG, PNG, BMP, PDF 및 TIFF
PDF 및 TIFF 파일의 경우 최대 2000페이지(프리 티어의 경우 처음 두 페이지만)가 처리됩니다.
이미지의 파일 크기는 500MB(프리 티어의 경우 4MB) 미만이어야 하며 크기는 최소 50 x 50픽셀, 최대 10000 x 10000픽셀이어야 합니다. PDF 파일에는 크기 제한이 없습니다.
추출할 텍스트의 최소 높이는 1024 x 768 이미지의 경우 12픽셀입니다. 이는 150 DPI에서 약 8개의 글꼴 포인트 텍스트에 해당합니다.

: Read API를 호출하여 이미지에서 텍스트를 추출하는 방법에 대한 가이드 

- 데이터 처리 방법 경정(선택 사항) 
- OCR 모델 지정 
- 입력 언어 
- 자연스러운 읽기 순서 출력(라틴어만 해당) 
- 서비스에 데이터 제출 
- 서비스에서 결과 얻기 
- 샘플 JSON 출력
- 코드 예시

```csharp
using System;
using System.Net.Http.Headers;
using System.Text;
using System.Net.Http;
using System.Web;

namespace CSHttpClientSample
{
    static class Program
    {
        static void Main()
        {
            MakeRequest();
            Console.WriteLine("Hit ENTER to exit...");
            Console.ReadLine();
        }
        
        static async void MakeRequest()
        {
            var client = new HttpClient();
            var queryString = HttpUtility.ParseQueryString(string.Empty);

            // Request headers
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "{subscription key}");

            // Request parameters
            queryString["language"] = "{string}";
            queryString["pages"] = "{string}";
            queryString["readingOrder"] = "{string}";
            queryString["model-version"] = "{string}";
            var uri = "https://westus.api.cognitive.microsoft.com/vision/v3.2/read/analyze?" + queryString;

            HttpResponseMessage response;

            // Request body
            byte[] byteData = Encoding.UTF8.GetBytes("{body}");

            using (var content = new ByteArrayContent(byteData))
            {
               content.Headers.ContentType = new MediaTypeHeaderValue("< your content type, i.e. application/json >");
               response = await client.PostAsync(uri, content);
            }

        }
    }
}
```

### Form Recognizer

- Invoice 필드 추출

| 이름 | 형식 | 설명 | 표준화된 출력 |
| --- | --- | --- | --- |
| CustomerName | String | 청구서가 청구된 고객 |  |
| CustomerId | String | 고객 참조 ID |  |
| PurchaseOrder | String | 구매 주문 참조 번호 |  |
| InvoiceId | String | 이 특정 청구서의 ID(많은 경우 ‘청구서 번호’) |  |
| InvoiceDate | Date | 청구서가 발행된 날짜 | yyyy-mm-dd |
| DueDate | Date | 이 청구서의 지불 기일 | yyyy-mm-dd |
| VendorName | String | 공급업체 이름 |  |
| VendorTaxId | String | 공급업체와 연결된 납세자 번호 |  |
| VendorAddress | String | 공급업체 우편 주소 |  |
| VendorAddressRecipient | String | VendorAddress와 관련된 이름 |  |
| CustomerAddress | String | 고객의 우편 주소 |  |
| CustomerTaxId | String | 고객과 연결된 납세자 번호 |  |
| CustomerAddressRecipient | String | CustomerAddress와 관련된 이름 |  |
| BillingAddress | String | 고객의 명시적 청구 주소 |  |
| BillingAddressRecipient | String | BillingAddress와 관련된 이름 |  |
| ShippingAddress | String | 고객의 명시적 배송 주소 |  |
| ShippingAddressRecipient | String | ShippingAddress와 관련된 이름 |  |
| PaymentTerm | String | 청구서에 대한 지불 조건 |  |
| 부분합 | 숫자 | 이 청구서에서 식별된 소계 필드 | 정수 |
| TotalTax | 숫자 | 이 청구서에서 식별된 세금 총액 필드 | 정수 |
| InvoiceTotal | 숫자(USD) | 이 청구서와 관련된 새 요금 총액 | 정수 |
| AmountDue | 숫자(USD) | 공급업체에 지불할 총액 | 정수 |
| ServiceAddress | String | 고객의 명시적 서비스 주소 또는 건물 주소 |  |
| ServiceAddressRecipient | String | ServiceAddress와 관련된 이름 |  |
| RemittanceAddress | String | 고객의 명시적 송금 주소 또는 지불 주소 |  |
| RemittanceAddressRecipient | String | RemittanceAddress와 관련된 이름 |  |
| ServiceStartDate | Date | 서비스 기간의 시작 날짜(예: 유틸리티 청구 서비스 기간) | yyyy-mm-dd |
| ServiceEndDate | Date | 서비스 기간의 종료 날짜(예: 유틸리티 청구 서비스 기간) | yyyy-mm-dd |
| PreviousUnpaidBalance | 숫자 | 이전에 미지불된 명시적 잔액 | 정수 |
| CurrencyCode | String | 추출된 금액과 연결된 통화 코드 |  |
| PaymentDetails | Array | 및 와 같은 IBAN결제 옵션 세부 정보를 보유하는 배열 SWIFT |  |
| TotalDiscount | 숫자 | 청구서에 적용된 총 할인 | 정수 |
| TaxItems(en-IN에만 해당) | Array | , IGST및 SGST와 같은 CGST추가 세금 정보를 보유하는 배열입니다. 이 줄 항목은 현재 인 로캘에만 사용할 수 있습니다. |  |
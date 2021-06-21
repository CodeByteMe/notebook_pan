# C#进行HTTP请求

```cs
using System;
using System.Text;
using System.Net;
using System.IO;
/*
C#进行HTTP请求
1 vscode使用C#
https://www.cnblogs.com/yilezhu/p/9926078.html
2 C#发送请求
https://www.cnblogs.com/wangxlei/p/11686013.html
3 C#添加请求头
https://www.cnblogs.com/yczz/archive/2012/06/01/2530484.html
*/
namespace HttpRequest
{
    class Program
    {
        static void Main(string[] args)
        {
            string jsonStr = getJSON();
            string tokenStr = getToken();
            string urlStr = "http://api.test.com/test";
            string result = putRequest(urlStr, tokenStr, jsonStr);
            Console.WriteLine(result);
        }

        private static string getJSON() {
            string jsonStr = "[{\"id\":\"5542\",\"age\":\"18\"}]";

            return jsonStr;
        }

        private static string getToken() {
            string token = "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhdWQiOiIzOWY0MGIzOS04ZWE1LTQ4NmItOWFlMy0yYWM4MjYxNDBhODEiLCJleHAiOjE2MDQ0MDU0MTksImlhdCI6MTYwNDM4NzQxOX0.DNYnlSZQfU2TXJhaKOXbfUFlfL7kI_y77ARut7_wNKI";

            return token;
        }

        private static string putRequest(string urlStr, string tokenStr, string jsonStr) {
            byte[] datas = Encoding.GetEncoding("UTF-8").GetBytes(jsonStr);
            HttpWebRequest request = (HttpWebRequest)HttpWebRequest.Create(urlStr);
            request.Method = "POST";
            request.Timeout = 150000;
            request.Headers.Add("token", tokenStr);
            request.AllowAutoRedirect = false;
            request.ContentType = "application/json";
            request.ContentLength = datas.Length;

            Stream requestStream = null;
            string response = "";

            requestStream = request.GetRequestStream();
            requestStream.Write(datas, 0, datas.Length);
            requestStream.Close();

            using (HttpWebResponse webResponse = (HttpWebResponse)request.GetResponse()) {
                Stream resposeStream = webResponse.GetResponseStream();
                byte[] outBytes = readFully(resposeStream);
                resposeStream.Close();
                response = Encoding.UTF8.GetString(outBytes);
            }

            return response;
        }

        private static byte[] readFully(Stream stream) {
            byte[] buffer = new byte[512];
            using (MemoryStream ms = new MemoryStream())
            {
                while (true)
                {
                    int read = stream.Read(buffer, 0, buffer.Length);
                    if (read <= 0)
                        return ms.ToArray();
                    ms.Write(buffer, 0, read);
                }
            }
        }
    }
}
```
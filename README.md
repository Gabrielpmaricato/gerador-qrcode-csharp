# Gerador de QRCode utilizando C#

Um guia passo a passo para gerar códigos QR gratuitamente.

## Introdução

Os códigos QR são amplamente adotados em aplicativos modernos para compartilhamento de links, informações de pagamento, autenticação e muito mais.

Com apenas algumas linhas de código, você pode gerar e salvar códigos QR como arquivos de imagem em sua máquina local. 

Essa abordagem funciona com .NET 8/.NET 9 e é totalmente <b>GRATUITA</b> , tornando-a ideal para projetos pessoais, demonstrações e soluções corporativas.

<h3>Etapa 1: instalar o pacote NuGet necessário</h3>

Execute o seguinte comando no diretório do seu projeto:

```csharp
dotnet add package QRCoder
```

Isso instala a biblioteca QRCoder que fornece vários renderizadores (PNG, Bitmap, SVG, etc.).

<h3>Etapa 2: Criar uma classe QRCodeHelper</h3>

Criaremos uma classe auxiliar para encapsular a geração de códigos QR. 

Para suporte multiplataforma, usaremos o PngByteQRCoderenderizador integrado, que retorna bytes PNG diretamente.

```csharp
using System;
using System.IO;
using QRCoder;

namespace MyPlaygroundApp.Util
{
    public static class QRCodeHelper
    {
        public static void GenerateToFile(string text, string filePath, int pixelsPerModule = 20)
        {
            using var qrGenerator = new QRCodeGenerator();
            using var qrData = qrGenerator.CreateQrCode(text, QRCodeGenerator.ECCLevel.Q);
            var qrCode = new PngByteQRCode(qrData);

            // Get PNG as byte array
            byte[] pngBytes = qrCode.GetGraphic(pixelsPerModule);

            // Save to file
            File.WriteAllBytes(filePath, pngBytes);
        }
    }
}
```

<h3>Etapa 3: Implementar o código do cliente</h3>

Agora, vamos chamar nosso auxiliar Main()para gerar um código QR para uma URL:

```csharp

using MyPlaygroundApp.Utils;

class Program
{
    static void Main(string[] args)
    {
        string url = "https://dev.to";

        string file = @"C:\Users\User\Downloads\devto.png";

        QRCodeHelper.GenerateToFile(url, file);

        Console.WriteLine($"QR code saved to {file}");
    }
}

```

Ao executar o programa, uma imagem de código QR será salva na pasta QRCodes relativa à saída do seu projeto.

<h3>Etapa 4: execute e veja o resultado</h3>

Crie e execute seu projeto:

```csharp
dotnet run
```

Verifique a pasta Downloads, você verá um arquivo qrcode.png.

Abra-o com qualquer visualizador de imagens ou digitalize-o com a câmera do seu celular. Você será redirecionado para https://dev.to .

Exemplo de código QR:

![QRCode gerado](assets/qrcode.jpg)

### Resumo

1. Usamos QRCoder, uma biblioteca gratuita e de código aberto, para gerar códigos QR em C#.
2. Criei uma QRCodeHelperclasse reutilizável.
3. Gerou e salvou um código QR como um arquivo PNG.
4. Com essa configuração, você pode facilmente estender o auxiliar para gerar códigos QR para credenciais de Wi-Fi, links de pagamento ou tokens de autenticação.

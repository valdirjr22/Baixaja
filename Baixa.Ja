<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Downloader do YouTube</title>
    <!-- Adiciona o Tailwind CSS para estilização fácil -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f3f4f6;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            padding: 1rem;
        }
        .container {
            max-width: 500px;
            width: 100%;
            background-color: #ffffff;
            padding: 2.5rem;
            border-radius: 1.5rem;
            box-shadow: 0 10px 25px -3px rgba(0, 0, 0, 0.1), 0 4px 10px -2px rgba(0, 0, 0, 0.05);
            text-align: center;
        }
        .loading-spinner {
            display: none;
            border: 4px solid rgba(0, 0, 0, 0.1);
            width: 36px;
            height: 36px;
            border-radius: 50%;
            border-left-color: #6366f1;
            animation: spin 1s ease infinite;
            margin: 0 auto;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1 class="text-3xl font-bold text-gray-900 mb-6">Downloader de Mídias do YouTube</h1>
        <p class="text-gray-600 mb-6">Cole o link do YouTube e baixe o vídeo ou o áudio.</p>
        
        <!-- Campo de entrada para o link do YouTube -->
        <input type="text" id="youtubeLink" placeholder="Cole o link do YouTube aqui..." class="w-full p-3 mb-4 text-gray-700 bg-gray-100 border-2 border-gray-300 rounded-lg focus:outline-none focus:border-indigo-500 transition-colors">
        
        <div class="flex flex-col sm:flex-row justify-center space-y-4 sm:space-y-0 sm:space-x-4">
            <!-- Botão para baixar o vídeo -->
            <button id="downloadVideoBtn" class="bg-indigo-600 text-white font-semibold py-3 px-6 rounded-lg shadow-md hover:bg-indigo-700 transition-colors">
                Baixar Vídeo
            </button>
            
            <!-- Botão para baixar o áudio -->
            <button id="downloadAudioBtn" class="bg-indigo-500 text-white font-semibold py-3 px-6 rounded-lg shadow-md hover:bg-indigo-600 transition-colors">
                Baixar Áudio
            </button>
        </div>
        
        <!-- Área para exibir mensagens de status e links de download -->
        <div id="statusMessage" class="mt-8 p-4 bg-gray-100 rounded-lg text-gray-800 hidden">
            <!-- As mensagens serão inseridas aqui via JavaScript -->
            <div class="loading-spinner" id="loadingSpinner"></div>
        </div>
        
        <!-- Script para a lógica da aplicação -->
        <script>
            // Adiciona event listeners aos botões
            document.getElementById('downloadVideoBtn').addEventListener('click', () => handleDownload('video'));
            document.getElementById('downloadAudioBtn').addEventListener('click', () => handleDownload('audio'));

            // Variável para armazenar o ID do download
            const appId = typeof __app_id !== 'undefined' ? __app_id : 'youtube-downloader-app-html-v2';

            async function handleDownload(tipo) {
                const link = document.getElementById('youtubeLink').value;
                const statusMessageElement = document.getElementById('statusMessage');
                const spinner = document.getElementById('loadingSpinner');
                
                // Valida o link do YouTube
                if (!link || !link.includes("youtube.com") && !link.includes("youtu.be")) {
                    statusMessageElement.innerHTML = `
                        <p class="text-red-500 font-semibold">❌ Por favor, insira um link válido do YouTube.</p>
                    `;
                    statusMessageElement.classList.remove('hidden');
                    return;
                }
                
                // Mostra a mensagem de "processando" e o spinner
                statusMessageElement.innerHTML = `<p class="text-gray-600 font-semibold">⏳ Processando o download de ${tipo}...</p>`;
                statusMessageElement.classList.remove('hidden');
                spinner.style.display = 'block';

                // Lógica de download
                // Em um ambiente real, esta parte faria uma chamada para um servidor (backend)
                // que teria a lógica para baixar e processar o arquivo.
                // Como estamos em um ambiente de cliente (HTML/JS),
                // simularemos a chamada a um servidor usando a API do Gemini.
                
                const prompt = `Simule uma resposta JSON de sucesso para um download de ${tipo} de um vídeo do YouTube. O título do vídeo é 'Exemplo de Música Pop'. A resposta deve ter a propriedade 'status': 'sucesso' e um 'download_link' fictício para o arquivo. O link original é '${link}'.`;
                
                let chatHistory = [];
                chatHistory.push({ role: "user", parts: [{ text: prompt }] });
                const payload = { 
                    contents: chatHistory,
                    generationConfig: {
                        responseMimeType: "application/json",
                        responseSchema: {
                            type: "OBJECT",
                            properties: {
                                "status": { "type": "STRING" },
                                "titulo": { "type": "STRING" },
                                "download_link": { "type": "STRING" }
                            }
                        }
                    }
                };

                const apiKey = "";
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;

                try {
                    const response = await fetch(apiUrl, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(payload)
                    });
                    
                    if (!response.ok) {
                        throw new Error(`Erro na API: ${response.statusText}`);
                    }
                    
                    const result = await response.json();
                    
                    const jsonResponse = JSON.parse(result.candidates[0].content.parts[0].text);
                    
                    if (jsonResponse.status === 'sucesso' && jsonResponse.download_link) {
                        const downloadLink = jsonResponse.download_link;
                        const titulo = jsonResponse.titulo;
                        
                        spinner.style.display = 'none';
                        statusMessageElement.innerHTML = `
                            <p class="text-green-600 font-semibold">✅ Download de ${tipo} concluído com sucesso!</p>
                            <p class="text-gray-500 mt-2">Título: '${titulo}'</p>
                            <div class="mt-4 flex flex-col sm:flex-row items-center justify-center space-y-2 sm:space-y-0 sm:space-x-2">
                                <a href="${downloadLink}" class="bg-gray-800 text-white py-2 px-4 rounded-lg hover:bg-gray-700 transition-colors" target="_blank" download>
                                    Baixar Arquivo
                                </a>
                                <button id="copyBtn" class="bg-gray-200 text-gray-800 py-2 px-4 rounded-lg hover:bg-gray-300 transition-colors">
                                    Copiar Link
                                </button>
                            </div>
                        `;

                        // Adiciona a funcionalidade de copiar para a área de transferência
                        document.getElementById('copyBtn').addEventListener('click', () => {
                            const tempInput = document.createElement('textarea');
                            tempInput.value = downloadLink;
                            document.body.appendChild(tempInput);
                            tempInput.select();
                            document.execCommand('copy');
                            document.body.removeChild(tempInput);
                            
                            const originalText = document.getElementById('copyBtn').innerText;
                            document.getElementById('copyBtn').innerText = 'Copiado!';
                            setTimeout(() => {
                                document.getElementById('copyBtn').innerText = originalText;
                            }, 2000);
                        });

                    } else {
                        throw new Error("Resposta do servidor com erro.");
                    }
                    
                } catch (error) {
                    spinner.style.display = 'none';
                    console.error("Erro durante o download simulado:", error);
                    statusMessageElement.innerHTML = `
                        <p class="text-red-500 font-semibold">❌ Ocorreu um erro no download. Tente novamente mais tarde.</p>
                    `;
                }
            }
        </script>
    </div>
</body>
</html>

 <!DOCTYPE html>
 <html lang="pt-BR">
 <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Meu Primeiro Quebra-Cabeça - Para Crianças</title>
     <script src="https://cdn.tailwindcss.com"></script>
     <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;700&display=swap" rel="stylesheet">
     <style>
         body {
             font-family: 'Inter', sans-serif;
             background-color: #e2e8f0; /* Fundo mais claro e suave */
             display: flex;
             justify-content: center;
             align-items: flex-start; /* Alinhar ao topo para mais espaço */
             min-height: 100vh;
             padding: 30px; /* Mais padding para um visual espaçoso */
             box-sizing: border-box;
         }
         .container {
             background-color: #ffffff;
             border-radius: 25px; /* Bordas mais arredondadas */
             box-shadow: 0 15px 30px rgba(0, 0, 0, 0.15); /* Sombra mais pronunciada */
             padding: 30px; /* Mais padding interno */
             display: flex;
             flex-direction: column;
             align-items: center;
             max-width: 1000px; /* Largura máxima um pouco maior */
             width: 100%;
             position: relative;
         }
         h1 {
             color: #2d3748; /* Cor de texto mais escura para o título */
             font-size: 2.5rem; /* Título maior */
             margin-bottom: 25px;
         }
         #canvas {
             border: 3px solid #cbd5e1; /* Borda mais visível */
             background-color: #ffffff;
             cursor: grab;
             touch-action: none;
             border-radius: 15px; /* Bordas arredondadas para o canvas */
             width: 100%;
             max-width: 900px;
             height: 600px;
             margin-bottom: 25px;
             box-shadow: 0 8px 15px rgba(0, 0, 0, 0.1); /* Sombra para o canvas */
         }
         #previewCanvas {
             position: absolute;
             bottom: 30px;
             right: 30px;
             border: 2px solid #a0aec0; /* Borda da prévia */
             background-color: #ffffff;
             width: 180px; /* Prévia um pouco maior */
             height: 120px;
             border-radius: 10px;
             box-shadow: 0 4px 10px rgba(0, 0, 0, 0.15); /* Sombra para a prévia */
             z-index: 10; /* Garante que a prévia fique por cima */
         }
         .controls {
             display: flex;
             flex-wrap: wrap;
             gap: 15px; /* Espaçamento maior entre controles */
             margin-bottom: 25px;
             justify-content: center;
             width: 100%;
             max-width: 900px;
         }
         .controls button, .controls input, .controls label {
             padding: 12px 20px; /* Padding maior para botões e inputs */
             border-radius: 12px; /* Bordas mais arredondadas */
             font-weight: 600; /* Texto um pouco mais bold */
             cursor: pointer;
             transition: all 0.3s ease-in-out; /* Transições mais suaves */
             box-shadow: 0 5px 12px rgba(0, 0, 0, 0.1); /* Sombra padrão */
             border: none;
             font-size: 1.1em; /* Fonte um pouco maior */
         }
         .controls button:hover {
             transform: translateY(-3px); /* Efeito de "levantar" */
             box-shadow: 0 8px 18px rgba(0, 0, 0, 0.2); /* Sombra maior no hover */
         }
         /* Cores e gradientes para botões */
         .bg-green-500 { background: linear-gradient(135deg, #4CAF50 0%, #66BB6A 100%); }
         .bg-green-500:hover { background: linear-gradient(135deg, #66BB6A 0%, #81C784 100%); }

         .bg-purple-500 { background: linear-gradient(135deg, #9C27B0 0%, #BA68C8 100%); }
         .bg-purple-500:hover { background: linear-gradient(135deg, #BA68C8 0%, #CE93D8 100%); }

         .bg-blue-500 { background: linear-gradient(135deg, #2196F3 0%, #64B5F6 100%); }
         .bg-blue-500:hover { background: linear-gradient(135deg, #64B5F6 0%, #90CAF9 100%); }

         input:is([type="file"]) {
             color: white;
             text-align: center;
         }
         input:is([type="text"], [type="number"]) {
             background-color: #f8fafc; /* Fundo claro para inputs */
             border: 2px solid #e2e8f0; /* Borda suave */
             color: #4a5568; /* Cor do texto do input */
             padding: 12px 18px;
             border-radius: 12px;
             min-width: 100px;
             font-size: 1em;
         }
         input:is([type="text"], [type="number"]):focus {
             outline: none;
             border-color: #63b3ed; /* Borda azul no foco */
             box-shadow: 0 0 0 3px rgba(99, 179, 237, 0.3); /* Sombra no foco */
         }
         .input-group {
             display: flex;
             gap: 15px;
             width: 100%;
             max-width: 900px;
             flex-wrap: wrap;
             justify-content: center;
         }
         .message-box {
             position: fixed;
             top: 50%;
             left: 50%;
             transform: translate(-50%, -50%);
             background-color: #4CAF50; /* Verde para sucesso */
             color: white;
             padding: 20px 30px; /* Padding maior */
             border-radius: 15px; /* Bordas mais arredondadas */
             box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
             font-size: 1.4em; /* Fonte maior */
             font-weight: bold;
             z-index: 1000;
             text-align: center;
             opacity: 0; /* Começa invisível */
             transition: opacity 0.5s ease-in-out; /* Transição para fade */
         }
         .message-box.show {
             opacity: 1; /* Torna visível */
         }
         .message-box.error {
             background-color: #f44336; /* Vermelho para erro */
         }

         #imageHistory {
             margin-top: 30px;
             width: 100%;
             max-width: 900px;
             background-color: #f8fafc;
             border-radius: 15px;
             padding: 20px;
             box-shadow: inset 0 2px 5px rgba(0, 0, 0, 0.05);
         }
         #imageHistory h2 {
             font-size: 1.8rem;
             color: #4a5568;
             margin-bottom: 15px;
             text-align: center;
         }
         #historyList {
             display: flex;
             flex-wrap: wrap;
             gap: 15px;
             justify-content: center;
         }
         .history-item {
             width: 120px;
             height: 90px;
             border: 2px solid #e2e8f0;
             border-radius: 8px;
             overflow: hidden;
             cursor: pointer;
             transition: all 0.2s ease-in-out;
             box-shadow: 0 2px 5px rgba(0, 0, 0, 0.08);
             position: relative;
         }
         .history-item:hover {
             transform: translateY(-2px) scale(1.05);
             box-shadow: 0 5px 12px rgba(0, 0, 0, 0.15);
             border-color: #63b3ed;
         }
         .history-item img {
             width: 100%;
             height: 100%;
             object-fit: cover;
             display: block;
         }
         .history-item .overlay {
             position: absolute;
             top: 0;
             left: 0;
             width: 100%;
             height: 100%;
             background-color: rgba(0, 0, 0, 0.5);
             color: white;
             display: flex;
             justify-content: center;
             align-items: center;
             font-size: 0.8em;
             text-align: center;
             opacity: 0;
             transition: opacity 0.2s ease-in-out;
         }
         .history-item:hover .overlay {
             opacity: 1;
         }
         #userIdDisplay {
             margin-top: 20px;
             font-size: 0.9em;
             color: #718096;
             text-align: center;
             width: 100%;
             word-break: break-all; /* Quebra a palavra se for muito longa */
         }
         #timerDisplay {
            font-size: 1.8rem;
            font-weight: bold;
            color: #2d3748;
            margin-bottom: 15px;
            text-align: center;
            width: 100%;
            padding: 10px;
            background-color: #f0f4f8;
            border-radius: 10px;
            box-shadow: inset 0 1px 3px rgba(0,0,0,0.1);
         }


         /* Estilos responsivos para mobile */
         @media (max-width: 768px) {
             body {
                 padding: 15px;
             }
             .container {
                 padding: 20px;
                 border-radius: 20px;
             }
             h1 {
                 font-size: 2rem;
                 margin-bottom: 20px;
             }
             #canvas {
                 height: 400px; /* Altura ajustada para mobile */
                 margin-bottom: 20px;
             }
             #previewCanvas {
                 width: 100px;
                 height: 70px;
                 bottom: 15px;
                 right: 15px;
             }
             .controls {
                 flex-direction: column;
                 align-items: stretch;
                 gap: 10px;
                 margin-bottom: 20px;
             }
             .controls button, .controls input, .controls label {
                 width: 100%;
                 font-size: 1em;
                 padding: 10px 15px;
             }
             .input-group {
                 flex-direction: column;
                 align-items: stretch;
                 gap: 10px;
             }
             #imageHistory {
                 padding: 15px;
             }
             #imageHistory h2 {
                 font-size: 1.5rem;
             }
             .history-item {
                 width: 100px;
                 height: 75px;
             }
             #timerDisplay {
                font-size: 1.4rem;
                padding: 8px;
             }
         }
     </style>
 </head>
 <body>
     <div class="container">
         <h1 class="text-3xl font-bold text-gray-800 mb-6">Quebra-Cabeça Divertido!</h1>

         <div class="controls">
             <label for="imageUpload" class="bg-green-500 text-white">
                 Carregar Imagem
                 <input type="file" id="imageUpload" accept="image/*" class="hidden">
             </label>
         </div>

         <div class="input-group mb-6">
             <input type="text" id="imageUrlInput" placeholder="Cole o URL da imagem aqui">
             <button id="loadImageFromUrlBtn" class="bg-purple-500 text-white">Carregar do URL</button>
         </div>

         <div class="controls mb-6">
             <label for="rowsInput" class="font-bold text-gray-700">Linhas:</label>
             <input type="number" id="rowsInput" value="3" min="2" max="10" class="w-20 text-center">
             <label for="colsInput" class="font-bold text-gray-700">Colunas:</label>
             <input type="number" id="colsInput" value="3" min="2" max="10" class="w-20 text-center">
             <label for="timeInput" class="font-bold text-gray-700">Tempo (minutos):</label>
             <input type="number" id="timeInput" value="5" min="1" max="10" class="w-24 text-center">
             <button id="startGameBtn" class="bg-blue-500 text-white">Iniciar Quebra-Cabeça</button>
         </div>

         <div id="timerDisplay">Tempo: 00:00</div>

         <canvas id="canvas"></canvas>
         <canvas id="previewCanvas"></canvas>

         <div id="imageHistory">
             <h2>Imagens Recentes</h2>
             <div id="historyList">
                 </div>
         </div>
         <div id="userIdDisplay" class="text-gray-600 mt-4"></div>
     </div>

     <script type="module">
         import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
         import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
         import { getFirestore, collection, addDoc, query, orderBy, onSnapshot, serverTimestamp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

         const canvas = document.getElementById('canvas');
         const ctx = canvas.getContext('2d');
         const previewCanvas = document.getElementById('previewCanvas');
         const previewCtx = previewCanvas.getContext('2d');
         const imageUpload = document.getElementById('imageUpload');
         const imageUrlInput = document.getElementById('imageUrlInput');
         const loadImageFromUrlBtn = document.getElementById('loadImageFromUrlBtn');
         const rowsInput = document.getElementById('rowsInput');
         const colsInput = document.getElementById('colsInput');
         const timeInput = document.getElementById('timeInput'); // New time input
         const startGameBtn = document.getElementById('startGameBtn');
         const historyList = document.getElementById('historyList');
         const userIdDisplay = document.getElementById('userIdDisplay');
         const timerDisplay = document.getElementById('timerDisplay');

         let puzzleImage = null; // A imagem carregada para o quebra-cabeça
         let puzzlePieces = []; // Array para armazenar as peças do quebra-cabeça
         let rows = 3;
         let cols = 3;
         let pieceWidth;
         let pieceHeight;

         let draggingPiece = null;
         let draggingPieceIndex = -1;
         let startMouseX;
         let startMouseY;
         let initialPieceX;
         let initialPieceY;

         // Variáveis para armazenar as dimensões e offsets da imagem desenhada
         let drawnImage = {
             width: 0,
             height: 0,
             offsetX: 0,
             offsetY: 0
         };

         // Timer variables
         let timerInterval = null;
         let timeRemaining = 0; // Tempo em segundos
         let gameActive = false; // Estado do jogo (ativo para interação)

         // Firebase Initialization
         const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
         const firebaseConfig = JSON.parse(typeof __firebase_config !== 'undefined' ? __firebase_config : '{}');
         const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;

         const app = initializeApp(firebaseConfig);
         const db = getFirestore(app);
         const auth = getAuth(app);

         let currentUserId = null;

         // Function to display messages on the screen (success/error)
         function showMessage(message, type = 'success') {
             const messageBox = document.createElement('div');
             messageBox.className = `message-box ${type}`;
             messageBox.textContent = message;
             document.body.appendChild(messageBox);

             // Force a reflow to ensure CSS transition works
             messageBox.offsetWidth;
             messageBox.classList.add('show'); // Add class to start fade-in transition

             setTimeout(() => {
                 messageBox.classList.remove('show'); // Remove class to start fade-out transition
                 messageBox.addEventListener('transitionend', () => {
                     messageBox.remove(); // Remove element from DOM after transition
                 }, { once: true }); // Ensure listener is removed after one execution
             }, 3000);
         }

         // Function to format time for display
         function formatTime(seconds) {
             const minutes = Math.floor(seconds / 60);
             const remainingSeconds = seconds % 60;
             const formattedMinutes = String(minutes).padStart(2, '0');
             const formattedSeconds = String(remainingSeconds).padStart(2, '0');
             return `${formattedMinutes}:${formattedSeconds}`;
         }

         // Function to start the timer
         function startTimer(durationInSeconds) {
             if (timerInterval) {
                 clearInterval(timerInterval);
             }
             timeRemaining = durationInSeconds;
             timerDisplay.textContent = `Tempo: ${formatTime(timeRemaining)}`;
             timerInterval = setInterval(() => {
                 timeRemaining--;
                 timerDisplay.textContent = `Tempo: ${formatTime(timeRemaining)}`;
                 if (timeRemaining <= 0) {
                     clearInterval(timerInterval);
                     gameActive = false; // Disable interaction
                     showMessage('Tempo Esgotado! O quebra-cabeça foi bloqueado.', 'error');
                     drawPuzzle(); // Redraw to remove active piece border if dragging
                 }
             }, 1000);
         }

         // Function to adjust the size of the main canvas and preview to be responsive
         function resizeCanvas() {
             const rect = canvas.getBoundingClientRect();
             canvas.width = rect.width;
             canvas.height = rect.height;
             ctx.imageSmoothingEnabled = true; // Ensure image smoothing is enabled
             drawPuzzle(); // Redraw the puzzle after resizing
             drawPreview(); // Redraw the preview as well
         }

         // Initialize canvas and adjust size
         window.addEventListener('load', () => {
             resizeCanvas();
         });

         // Add a listener to resize the canvas when the window is resized
         window.addEventListener('resize', resizeCanvas);

         // Function to load an image
         function loadImage(src, fromHistory = false) {
             return new Promise((resolve, reject) => {
                 const img = new Image();
                 img.crossOrigin = 'Anonymous'; // Important to avoid CORS issues
                 img.onload = () => {
                     puzzleImage = img;
                     if (!fromHistory) { // Only show success message and save to history if not loaded from history
                         showMessage('Imagem carregada com sucesso!', 'success');
                         saveImageToHistory(src);
                     }
                     // Clear the canvas and draw the full image for visualization
                     ctx.clearRect(0, 0, canvas.width, canvas.height);
                     drawFullImage(img);
                     drawPreview(); // Draw the preview after loading the image
                     resolve(img);
                 };
                 img.onerror = () => {
                     puzzleImage = null;
                     showMessage('Não foi possível carregar a imagem. Verifique o URL ou se a imagem permite carregamento externo (CORS).', 'error');
                     reject(new Error('Falha ao carregar imagem.'));
                 };
                 img.src = src;
             });
         }

         // Draw the full image on the canvas, adjusting to fit (CONTAIN logic)
         function drawFullImage(img, targetCtx = ctx) {
             targetCtx.clearRect(0, 0, targetCtx.canvas.width, targetCtx.canvas.height);
             let imgWidth = img.width;
             let imgHeight = img.height;
             const canvasAspectRatio = targetCtx.canvas.width / targetCtx.canvas.height;
             const imgAspectRatio = imgWidth / imgHeight;

             let scale;
             if (imgAspectRatio > canvasAspectRatio) {
                 // Image is wider in relation to its height than the canvas
                 // Scale based on width to fit horizontally
                 scale = targetCtx.canvas.width / imgWidth;
             } else {
                 // Image is taller in relation to its width than the canvas
                 // Scale based on height to fit vertically
                 scale = targetCtx.canvas.height / imgHeight;
             }

             let drawWidth = imgWidth * scale;
             let drawHeight = imgHeight * scale;
             let offsetX = (targetCtx.canvas.width - drawWidth) / 2;
             let offsetY = (targetCtx.canvas.height - drawHeight) / 2;

             targetCtx.drawImage(img, offsetX, offsetY, drawWidth, drawHeight);

             // If drawing on the main canvas, store the dimensions and offsets
             if (targetCtx === ctx) {
                 drawnImage.width = drawWidth;
                 drawnImage.height = drawHeight;
                 drawnImage.offsetX = offsetX;
                 drawnImage.offsetY = offsetY;
             }
         }

         // Function to draw the grid over the image on a specific context
         function drawGridOnContext(context, currentRows, currentCols, currentPieceWidth, currentPieceHeight, offsetX = 0, offsetY = 0, totalWidth, totalHeight) {
             context.strokeStyle = '#000000'; // Grid color (black)
             context.lineWidth = 1; // Grid line thickness

             for (let r = 0; r <= currentRows; r++) {
                 context.beginPath();
                 context.moveTo(offsetX, offsetY + r * currentPieceHeight);
                 context.lineTo(offsetX + totalWidth, offsetY + r * currentPieceHeight);
                 context.stroke();
             }

             for (let c = 0; c <= currentCols; c++) {
                 context.beginPath();
                 context.moveTo(offsetX + c * currentPieceWidth, offsetY);
                 context.lineTo(offsetX + c * currentPieceWidth, offsetY + totalHeight);
                 context.stroke();
             }
         }

         // Function to draw the divided image in the preview
         function drawPreview() {
             if (puzzleImage) {
                 const previewRows = parseInt(rowsInput.value);
                 const previewCols = parseInt(colsInput.value);
                 const previewPieceWidth = previewCanvas.width / previewCols;
                 const previewPieceHeight = previewCanvas.height / previewRows;

                 previewCtx.clearRect(0, 0, previewCanvas.width, previewCanvas.height);
                 drawFullImage(puzzleImage, previewCtx);
                 drawGridOnContext(previewCtx, previewRows, previewCols, previewPieceWidth, previewPieceHeight, 0, 0, previewCanvas.width, previewCanvas.height);
             } else {
                 previewCtx.clearRect(0, 0, previewCanvas.width, previewCanvas.height);
             }
         }

         // Event listener for loading image from computer
         imageUpload.addEventListener('change', (e) => {
             const file = e.target.files && e.target.files.length > 0 ? e.target.files[0] : null;
             if (file) {
                 const reader = new FileReader();
                 reader.onload = (event) => {
                     loadImage(event.target.result);
                 };
                 reader.readAsDataURL(file);
             }
         });

         // Event listener for loading image from URL
         loadImageFromUrlBtn.addEventListener('click', () => {
             const imageUrl = imageUrlInput.value.trim();
             if (imageUrl) {
                 loadImage(imageUrl);
             } else {
                 showMessage('Por favor, insira um URL de imagem válido.', 'error');
             }
         });

         // Function to create puzzle pieces
         function createPuzzlePieces() {
             if (!puzzleImage) {
                 showMessage('Por favor, carregue uma imagem primeiro!', 'error');
                 return;
             }

             rows = parseInt(rowsInput.value);
             cols = parseInt(colsInput.value);
             let selectedTimeMinutes = parseInt(timeInput.value); // Get time in minutes

             if (isNaN(rows) || isNaN(cols) || rows < 2 || cols < 2) {
                 showMessage('Número de linhas e colunas deve ser no mínimo 2.', 'error');
                 return;
             }
             if (isNaN(selectedTimeMinutes) || selectedTimeMinutes < 1) { // Min 1 minute
                 showMessage('O tempo deve ser um número válido (mínimo 1 minuto).', 'error');
                 return;
             }
             let selectedTimeSeconds = selectedTimeMinutes * 60; // Convert to seconds


             // Calculate the size of each piece based on the drawn image dimensions
             pieceWidth = drawnImage.width / cols;
             pieceHeight = drawnImage.height / rows;

             // 1. Draw the full image on the main canvas (already updates drawnImage)
             drawFullImage(puzzleImage);
             // 2. Draw the grid over the full image
             drawGridOnContext(ctx, rows, cols, pieceWidth, pieceHeight, drawnImage.offsetX, drawnImage.offsetY, drawnImage.width, drawnImage.height);
             // 3. Draw the preview
             drawPreview();

             // Small delay for the child to see the image with the grid before shuffling
             setTimeout(() => {
                 puzzlePieces = [];
                 // Create a temporary canvas to cut the pieces with smoothing
                 const tempCanvas = document.createElement('canvas');
                 const tempCtx = tempCanvas.getContext('2d');
                 tempCtx.imageSmoothingEnabled = true;

                 // Adjust the size of the temporary canvas to the dimensions of the drawn image
                 tempCanvas.width = drawnImage.width;
                 tempCanvas.height = drawnImage.height;

                 // Draw only the image content on the tempCanvas, without offsets
                 tempCtx.drawImage(puzzleImage, 0, 0, drawnImage.width, drawnImage.height);


                 // Cut the pieces from the tempCanvas
                 for (let r = 0; r < rows; r++) {
                     for (let c = 0; c < cols; c++) {
                         const x = c * pieceWidth;
                         const y = r * pieceHeight;

                         // Get the image data for this piece from the tempCanvas
                         const pieceImageData = tempCtx.getImageData(x, y, pieceWidth, pieceHeight);

                         puzzlePieces.push({
                             imageData: pieceImageData,
                             originalRow: r,
                             originalCol: c,
                             currentRow: r, // Initial position (for shuffling later)
                             currentCol: c,
                             // Initial drawing position on the main canvas, considering the image offset
                             x: drawnImage.offsetX + (c * pieceWidth),
                             y: drawnImage.offsetY + (r * pieceHeight)
                         });
                     }
                 }

                 shufflePieces(); // Shuffle the pieces
                 drawPuzzle(); // Draw the shuffled puzzle
                 showMessage('Quebra-cabeça iniciado! Arraste as peças para montar.', 'success');
                 gameActive = true; // Enable game interaction
                 startTimer(selectedTimeSeconds); // Start timer with user-selected time in seconds
             }, 500); // 0.5 second delay for visualization
         }

         // Function to shuffle puzzle pieces
         function shufflePieces() {
             for (let i = puzzlePieces.length - 1; i > 0; i--) {
                 const j = Math.floor(Math.random() * (i + 1));
                 const temp = puzzlePieces[i];
                 puzzlePieces[i] = puzzlePieces[j];
                 puzzlePieces[j] = temp;
             }

             // Update current positions of pieces after shuffling
             puzzlePieces.forEach((piece, index) => {
                 piece.currentRow = Math.floor(index / cols);
                 piece.currentCol = index % cols;
                 // Update x, y based on new currentRow/Col and drawn image offsets
                 piece.x = drawnImage.offsetX + (piece.currentCol * pieceWidth);
                 piece.y = drawnImage.offsetY + (piece.currentRow * pieceHeight);
             });
         }

         // Function to draw all pieces on the canvas
         function drawPuzzle() {
             ctx.clearRect(0, 0, canvas.width, canvas.height); // Clear the canvas

             if (!puzzleImage) {
                 // If no image is loaded, just clear the canvas and return
                 return;
             }

             if (puzzlePieces.length === 0) {
                 // If the image is loaded but the puzzle has not started, draw the full image.
                 // Also draw the grid for visualization before the puzzle starts.
                 drawFullImage(puzzleImage);

                 // Calculate temporary piece dimensions for grid display
                 const tempPieceWidth = drawnImage.width / cols;
                 const tempPieceHeight = drawnImage.height / rows;

                 drawGridOnContext(ctx, rows, cols, tempPieceWidth, tempPieceHeight, drawnImage.offsetX, drawnImage.offsetY, drawnImage.width, drawnImage.height);
                 return;
             }

             puzzlePieces.forEach(piece => {
                 // Draw the piece only if image data is defined
                 if (piece.imageData) {
                     ctx.putImageData(piece.imageData, piece.x, piece.y);

                     // Draw a border for each piece (optional, for visualization)
                     ctx.strokeStyle = '#ccc';
                     ctx.lineWidth = 1;
                     ctx.strokeRect(piece.x, piece.y, pieceWidth, pieceHeight);
                 }
             });

             // If dragging, draw the dragged piece on top for visibility
             if (draggingPiece && draggingPiece.imageData) { // Also check if draggingPiece.imageData is defined
                 ctx.putImageData(draggingPiece.imageData, draggingPiece.x, draggingPiece.y);
                 ctx.strokeStyle = '#3b82f6'; // Blue border for the dragged piece
                 ctx.lineWidth = 2;
                 ctx.strokeRect(draggingPiece.x, draggingPiece.y, pieceWidth, pieceHeight);
             }
         }

         // Event listener to start the game
         startGameBtn.addEventListener('click', createPuzzlePieces);

         // --- Drag and Drop Logic ---
         function getPieceAtMouse(e) {
             if (!gameActive) return null; // Prevent interaction if game is not active

             const rect = canvas.getBoundingClientRect();
             // Access clientX/Y directly or check e.touches.length before accessing e.touches[0]
             const clientX = e.clientX || (e.touches && e.touches.length > 0 ? e.touches[0].clientX : 0);
             const clientY = e.clientY || (e.touches && e.touches.length > 0 ? e.touches[0].clientY : 0);

             const mouseX = clientX - rect.left;
             const mouseY = clientY - rect.top;

             // Adjust mouse coordinates relative to the drawn image area
             const relativeX = mouseX - drawnImage.offsetX;
             const relativeY = mouseY - drawnImage.offsetY;

             // Check if the click is within the puzzle area
             if (relativeX < 0 || relativeX >= drawnImage.width || relativeY < 0 || relativeY >= drawnImage.height) {
                 return null; // Click outside the puzzle area
             }

             for (let i = 0; i < puzzlePieces.length; i++) {
                 const piece = puzzlePieces[i];
                 // Check the current drawing position of the piece
                 if (mouseX >= piece.x && mouseX < piece.x + pieceWidth &&
                     mouseY >= piece.y && mouseY < piece.y + pieceHeight) {
                     return { piece: piece, index: i };
                 }
             }
             return null;
         }

         canvas.addEventListener('mousedown', (e) => {
             if (!gameActive) return;
             const result = getPieceAtMouse(e);
             if (result) {
                 draggingPiece = result.piece;
                 draggingPieceIndex = result.index;
                 startMouseX = e.clientX || (e.touches && e.touches.length > 0 ? e.touches[0].clientX : 0);
                 startMouseY = e.clientY || (e.touches && e.touches.length > 0 ? e.touches[0].clientY : 0);
                 initialPieceX = draggingPiece.x;
                 initialPieceY = draggingPiece.y;
                 canvas.style.cursor = 'grabbing'; // Change cursor
             }
         });

         canvas.addEventListener('mousemove', (e) => {
             if (!gameActive) return;
             if (draggingPiece) {
                 const currentMouseX = e.clientX || (e.touches && e.touches.length > 0 ? e.touches[0].clientX : 0);
                 const currentMouseY = e.clientY || (e.touches && e.touches.length > 0 ? e.touches[0].clientY : 0);

                 draggingPiece.x = initialPieceX + (currentMouseX - startMouseX);
                 draggingPiece.y = initialPieceY + (currentMouseY - startMouseY);
                 drawPuzzle();
             }
         });

         canvas.addEventListener('mouseup', (e) => {
             if (!gameActive) return;
             if (draggingPiece) {
                 canvas.style.cursor = 'grab'; // Restore cursor

                 const rect = canvas.getBoundingClientRect();
                 const dropX = (e.clientX || (e.changedTouches && e.changedTouches.length > 0 ? e.changedTouches[0].clientX : 0)) - rect.left;
                 const dropY = (e.clientY || (e.changedTouches && e.changedTouches.length > 0 ? e.changedTouches[0].clientY : 0)) - rect.top;

                 // Calculate the target position relative to the drawn image area
                 const relativeDropX = dropX - drawnImage.offsetX;
                 const relativeDropY = dropY - drawnImage.offsetY;

                 // Calculate the target cell based on relative coordinates
                 const targetCol = Math.floor(relativeDropX / pieceWidth);
                 const targetRow = Math.floor(relativeDropY / pieceHeight);

                 // Ensure the target cell is within bounds
                 const finalCol = Math.max(0, Math.min(cols - 1, targetCol));
                 const finalRow = Math.max(0, Math.min(rows - 1, targetRow));

                 // Find the piece that is in the target cell
                 let targetPiece = null;
                 let targetPieceIndex = -1;
                 for (let i = 0; i < puzzlePieces.length; i++) {
                     if (puzzlePieces[i].currentRow === finalRow && puzzlePieces[i].currentCol === finalCol) {
                         targetPiece = puzzlePieces[i];
                         targetPieceIndex = i;
                         break;
                     }
                 }

                 if (targetPiece && targetPiece !== draggingPiece) {
                     // Swap logical positions (currentRow, currentCol)
                     const tempRow = draggingPiece.currentRow;
                     const tempCol = draggingPiece.currentCol;

                     draggingPiece.currentRow = targetPiece.currentRow;
                     draggingPiece.currentCol = targetPiece.currentCol;
                     targetPiece.currentRow = tempRow;
                     targetPiece.currentCol = tempCol;

                     // Swap pieces in the array to maintain logical order
                     puzzlePieces[draggingPieceIndex] = targetPiece;
                     puzzlePieces[targetPieceIndex] = draggingPiece;

                     // Update drawing positions (x, y) for both pieces
                     draggingPiece.x = drawnImage.offsetX + (draggingPiece.currentCol * pieceWidth);
                     draggingPiece.y = drawnImage.offsetY + (draggingPiece.currentRow * pieceHeight);
                     targetPiece.x = drawnImage.offsetX + (targetPiece.currentCol * pieceWidth);
                     targetPiece.y = drawnImage.offsetY + (targetPiece.currentRow * pieceHeight);

                 } else {
                     // If no target piece or it's the same piece, just adjust the position
                     draggingPiece.currentRow = finalRow;
                     draggingPiece.currentCol = finalCol;
                     draggingPiece.x = drawnImage.offsetX + (finalCol * pieceWidth);
                     draggingPiece.y = drawnImage.offsetY + (finalRow * pieceHeight);
                 }

                 draggingPiece = null;
                 draggingPieceIndex = -1;
                 drawPuzzle();
                 checkCompletion();
             }
         });

         canvas.addEventListener('mouseout', () => {
             if (!gameActive) return;
             if (draggingPiece) {
                 // If the mouse leaves the canvas while dragging, drop the piece at the current position
                 canvas.style.cursor = 'grab';
                 draggingPiece = null;
                 draggingPieceIndex = -1;
                 drawPuzzle();
             }
         });

         // --- Touch Logic for Drag and Drop ---
         canvas.addEventListener('touchstart', (e) => {
             if (!gameActive) return;
             e.preventDefault(); // Prevent page scroll
             const result = getPieceAtMouse(e);
             if (result) {
                 draggingPiece = result.piece;
                 draggingPieceIndex = result.index;
                 startMouseX = e.touches[0].clientX;
                 startMouseY = e.touches[0].clientY;
                 initialPieceX = draggingPiece.x;
                 initialPieceY = draggingPiece.y;
             }
         }, { passive: false });

         canvas.addEventListener('touchmove', (e) => {
             if (!gameActive) return;
             e.preventDefault(); // Prevent page scroll
             if (draggingPiece) {
                 const currentMouseX = e.touches[0].clientX;
                 const currentMouseY = e.touches[0].clientY;

                 draggingPiece.x = initialPieceX + (currentMouseX - startMouseX);
                 draggingPiece.y = initialPieceY + (currentMouseY - startMouseY);
                 drawPuzzle();
             }
         }, { passive: false });

         canvas.addEventListener('touchend', (e) => {
             if (!gameActive) return;
             if (draggingPiece) {
                 const rect = canvas.getBoundingClientRect();
                 const dropX = e.changedTouches[0].clientX - rect.left;
                 const dropY = e.changedTouches[0].clientY - rect.top;

                 const relativeDropX = dropX - drawnImage.offsetX;
                 const relativeDropY = dropY - drawnImage.offsetY;

                 const targetCol = Math.floor(relativeDropX / pieceWidth);
                 const targetRow = Math.floor(relativeDropY / pieceHeight);

                 const finalCol = Math.max(0, Math.min(cols - 1, targetCol));
                 const finalRow = Math.max(0, Math.min(rows - 1, targetRow));

                 let targetPiece = null;
                 let targetPieceIndex = -1;
                 for (let i = 0; i < puzzlePieces.length; i++) {
                     if (puzzlePieces[i].currentRow === finalRow && puzzlePieces[i].currentCol === finalCol) {
                         targetPiece = puzzlePieces[i];
                         targetPieceIndex = i;
                         break;
                     }
                 }

                 if (targetPiece && targetPiece !== draggingPiece) {
                     const tempRow = draggingPiece.currentRow;
                     const tempCol = draggingPiece.currentCol;

                     draggingPiece.currentRow = targetPiece.currentRow;
                     draggingPiece.currentCol = targetPiece.currentCol;
                     targetPiece.currentRow = tempRow;
                     targetPiece.currentCol = tempCol;

                     puzzlePieces[draggingPieceIndex] = targetPiece;
                     puzzlePieces[targetPieceIndex] = draggingPiece;

                     draggingPiece.x = drawnImage.offsetX + (draggingPiece.currentCol * pieceWidth);
                     draggingPiece.y = drawnImage.offsetY + (draggingPiece.currentRow * pieceHeight);
                     targetPiece.x = drawnImage.offsetX + (targetPiece.currentCol * pieceWidth);
                     targetPiece.y = drawnImage.offsetY + (targetPiece.currentRow * pieceHeight);

                 } else {
                     draggingPiece.currentRow = finalRow;
                     draggingPiece.currentCol = finalCol;
                     draggingPiece.x = drawnImage.offsetX + (finalCol * pieceWidth);
                     draggingPiece.y = drawnImage.offsetY + (finalRow * pieceHeight);
                 }

                 draggingPiece = null;
                 draggingPieceIndex = -1;
                 drawPuzzle();
                 checkCompletion();
             }
         });

         // Function to check if the puzzle is complete
         function checkCompletion() {
             const isComplete = puzzlePieces.every(piece =>
                 piece.currentRow === piece.originalRow && piece.currentCol === piece.originalCol
             );

             if (isComplete) {
                 // Draw the full image without piece borders upon completion
                 drawFullImage(puzzleImage);
                 showMessage('Parabéns! Você montou o quebra-cabeça!', 'success');
                 clearInterval(timerInterval); // Stop the timer on completion
                 gameActive = false; // Disable further interaction
             }
         }

         // --- Firebase Logic ---
         // Listen for authentication state changes
         onAuthStateChanged(auth, async (user) => {
             if (user) {
                 currentUserId = user.uid;
                 console.log("Usuário autenticado:", currentUserId);
                 userIdDisplay.textContent = `ID do Usuário: ${currentUserId}`;
                 // Now that we have a user, load the image history
                 loadImageHistory();
             } else {
                 try {
                     if (initialAuthToken) {
                         await signInWithCustomToken(auth, initialAuthToken);
                     } else {
                         await signInAnonymously(auth);
                     }
                 } catch (error) {
                     console.error("Erro ao autenticar no Firebase:", error);
                     // Fallback if anonymous sign-in also fails (e.g., no internet)
                     currentUserId = crypto.randomUUID(); // Use a random ID if auth fails
                     userIdDisplay.textContent = `ID do Usuário (offline): ${currentUserId}`;
                     showMessage('Erro ao conectar ao Firebase. Histórico de imagens pode não ser salvo.', 'error');
                 }
             }
         });

         // Function to save image URL/dataURL to Firestore
         async function saveImageToHistory(imageUrl) {
             if (!currentUserId) {
                 console.warn("Usuário não autenticado. Não é possível salvar o histórico de imagens.");
                 return;
             }
             try {
                 const imageHistoryRef = collection(db, `artifacts/${appId}/users/${currentUserId}/imageHistory`);
                 await addDoc(imageHistoryRef, {
                     imageUrl: imageUrl,
                     timestamp: serverTimestamp() // Use serverTimestamp for consistent time
                 });
                 console.log("Imagem salva no histórico com sucesso!");
             } catch (e) {
                 console.error("Erro ao adicionar documento: ", e);
                 showMessage('Erro ao salvar imagem no histórico.', 'error');
             }
         }

         // Function to load image history from Firestore
         function loadImageHistory() {
             if (!currentUserId) {
                 console.warn("Usuário não autenticado. Não é possível carregar o histórico de imagens.");
                 return;
             }

             const imageHistoryRef = collection(db, `artifacts/${appId}/users/${currentUserId}/imageHistory`);
             const q = query(imageHistoryRef, orderBy("timestamp", "desc")); // Order by most recent

             onSnapshot(q, (snapshot) => {
                 historyList.innerHTML = ''; // Clear current history list
                 snapshot.forEach((doc) => {
                     const imageData = doc.data();
                     const imageUrl = imageData.imageUrl;

                     const historyItem = document.createElement('div');
                     historyItem.className = 'history-item';
                     historyItem.innerHTML = `<img src="${imageUrl}" alt="Imagem do Histórico" onerror="this.src='https://placehold.co/120x90/cccccc/333333?text=Erro'">
                                              <div class="overlay">Carregar</div>`;
                     historyItem.addEventListener('click', () => {
                         loadImage(imageUrl, true); // Load image from history, prevent saving again
                     });
                     historyList.appendChild(historyItem);
                 });
             }, (error) => {
                 console.error("Erro ao carregar histórico de imagens: ", error);
                 showMessage('Erro ao carregar histórico de imagens.', 'error');
             });
         }
     </script>
 </body>
 </html>
 

<html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Match the Verb with the Picture</title>
    <!-- Load Tailwind CSS for styling -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Custom CSS for base styles and specific game elements */
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f0f4f8; /* Light blue-gray background */
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }
        .container {
            background-color: #ffffff;
            padding: 30px;
            border-radius: 16px; /* Rounded corners for the main container */
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1); /* Soft shadow */
            text-align: center;
            max-width: 900px;
            width: 100%;
        }
        .title {
            font-size: 2.25rem; /* Large title font size */
            font-weight: 700; /* Bold font weight */
            color: #2c3e50; /* Dark blue-gray text color */
            margin-bottom: 25px;
        }
        .score-display {
            font-size: 1.2rem;
            font-weight: 600;
            color: #34495e;
            margin-bottom: 15px;
        }
        .game-area {
            display: flex;
            flex-direction: column; /* Stack vertically on small screens */
            gap: 20px;
        }
        /* Media query for larger screens (md: breakpoint and up) */
        @media (min-width: 768px) {
            .game-area {
                flex-direction: row; /* Arrange horizontally on larger screens */
                justify-content: space-around;
            }
        }
        .images-section, .verbs-section {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 15px;
            width: 100%; /* Take full width on small screens */
        }
        /* Styling for the image containers (now holding emojis) */
        .image-container {
            background-color: #ecf0f1; /* Light gray background */
            border: 2px dashed #bdc3c7; /* Dashed border for drop target feel */
            border-radius: 12px;
            padding: 10px;
            width: 100%; /* Make width fluid */
            max-width: 160px; /* Set a max-width for image containers */
            aspect-ratio: 1 / 1; /* Maintain square aspect ratio */
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            position: relative;
            cursor: grab;
            overflow: hidden;
            font-size: 4rem; /* Large font size for emoji */
        }
        .emoji-display {
            font-size: 4rem; /* Specific font size for the emoji */
            line-height: 1; /* Adjust line height to prevent extra space */
            margin-bottom: 10px; /* Space between emoji and verb label */
        }
        .image-container.has-verb {
            border: 2px solid #2ecc71; /* Green border for correctly matched images */
        }
        .image-container.drag-over {
            background-color: #d0d7dd; /* Slightly darker background on drag over */
            border-color: #3498db; /* Blue border on drag over */
        }
        /* Styling for the label of the dropped verb */
        .verb-label {
            position: absolute;
            bottom: 20px; /* Adjusted to make space for individual feedback */
            background-color: #34495e; /* Dark blue-gray background */
            color: white;
            padding: 4px 8px;
            border-radius: 6px;
            font-size: 0.85rem;
            pointer-events: none; /* Allows click-through to underlying elements */
        }
        /* Styling for individual feedback messages under each image */
        .individual-feedback {
            position: absolute;
            bottom: 5px; /* Position below the verb label */
            font-size: 0.9rem; /* Smaller font for individual feedback */
            font-weight: 600;
            min-height: 1.2rem; /* Ensures space even when empty */
        }
        .individual-feedback.correct {
            color: #2ecc71; /* Green for correct feedback */
        }
        .individual-feedback.incorrect {
            color: #e74c3c; /* Red for incorrect feedback */
        }

        /* Styling for verb cards */
        .verb-card {
            background-color: #3498db; /* Blue background */
            color: white;
            padding: 12px 20px;
            border-radius: 10px;
            cursor: grab;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            transition: transform 0.2s ease-in-out;
            width: 100%; /* Fluid width */
            max-width: 130px; /* Max width for verb cards */
            text-align: center;
            font-weight: 600;
        }
        .verb-card:active {
            transform: scale(1.05); /* Slight scale effect on active */
        }
        .verb-card.dragging {
            opacity: 0.7; /* Reduce opacity when dragging */
        }
        .verb-card.hidden {
            display: none; /* Hide matched verb cards */
        }
        /* Styling for button group */
        .button-group {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin-top: 25px;
        }
        .button {
            background-color: #2c3e50; /* Dark blue-gray button */
            color: white;
            padding: 12px 25px;
            border-radius: 10px;
            border: none;
            cursor: pointer;
            font-size: 1rem;
            font-weight: 600;
            transition: background-color 0.3s ease, transform 0.2s ease;
        }
        .button:hover {
            background-color: #34495e; /* Slightly lighter on hover */
            transform: translateY(-2px); /* Slight lift on hover */
        }
        .button:active {
            transform: translateY(0); /* Return to original position on click */
        }
        /* Modal styling */
        .modal {
            display: none; /* Hidden by default */
            position: fixed; /* Stay in place */
            z-index: 1000; /* Sit on top of everything */
            left: 0;
            top: 0;
            width: 100%; /* Full width */
            height: 100%; /* Full height */
            overflow: auto; /* Enable scroll if needed */
            background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
            justify-content: center;
            align-items: center;
        }
        .modal-content {
            background-color: #fefefe;
            margin: auto;
            padding: 20px;
            border: 1px solid #888;
            width: 90%; /* Responsive width */
            max-width: 500px; /* Max width for larger screens */
            border-radius: 10px;
            text-align: center;
            box-shadow: 0 5px 15px rgba(0,0,0,0.3);
            position: relative;
        }
        .close-button {
            color: #aaa;
            position: absolute;
            top: 10px;
            right: 15px;
            font-size: 28px;
            font-weight: bold;
            cursor: pointer;
        }
        .close-button:hover,
        .close-button:focus {
            color: black;
            text-decoration: none;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="title">Match the Verb with the Picture</div>
        <div id="scoreDisplay" class="score-display">Score: 0</div>
        <div class="game-area">
            <div class="images-section grid grid-cols-2 sm:grid-cols-3 gap-4 md:flex md:flex-row md:flex-wrap md:justify-center">
                <!-- Image containers will be dynamically loaded here -->
            </div>
            <div class="verbs-section grid grid-cols-2 sm:grid-cols-3 gap-4 md:flex md:flex-row md:flex-wrap md:justify-center">
                <!-- Verb cards will be dynamically loaded here -->
            </div>
        </div>
        <div class="button-group">
            <button id="restartButton" class="button">Restart</button>
            <button id="nextButton" class="button hidden">Next</button>
        </div>
    </div>

    <!-- The Modal for messages -->
    <div id="messageModal" class="modal">
        <div class="modal-content">
            <span class="close-button">&times;</span>
            <p id="modalMessage" class="py-4 text-lg"></p>
        </div>
    </div>

    <script>
        // Array of game data: emoji and correct verb
        const gameData = [
            { id: 'img1', emoji: '🏃‍♀️', verb: 'run' },
            { id: 'img2', emoji: '📖', verb: 'read' },
            { id: 'img3', emoji: '🍔', verb: 'eat' },
            { id: 'img4', emoji: '⚽', verb: 'play' },
            { id: 'img5', emoji: '🤸‍♀️', verb: 'jump' },
            { id: 'img6', emoji: '💃', verb: 'dance' }
        ];

        let draggedVerb = null; // Stores the verb DOM element being dragged
        let matchedPairs = 0;   // Counts the number of correctly matched pairs
        let score = 0;          // Tracks the player's score

        // DOM element references
        const imagesSection = document.querySelector('.images-section');
        const verbsSection = document.querySelector('.verbs-section');
        const nextButton = document.getElementById('nextButton');
        const restartButton = document.getElementById('restartButton');
        const scoreDisplay = document.getElementById('scoreDisplay');
        const messageModal = document.getElementById('messageModal');
        const modalMessage = document.getElementById('modalMessage');
        const closeButton = document.querySelector('.close-button');

        /**
         * Shuffles an array in place using the Fisher-Yates (Knuth) algorithm.
         * @param {Array} array The array to shuffle.
         * @returns {Array} The shuffled array.
         */
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }

        /**
         * Updates the score displayed on the screen.
         */
        function updateScoreDisplay() {
            scoreDisplay.textContent = `Score: ${score}`;
        }

        /**
         * Initializes the game board by clearing previous elements,
         * shuffling game data, and creating new emoji containers and verb cards.
         */
        function initializeGame() {
            imagesSection.innerHTML = ''; // Clear previous images/emojis
            verbsSection.innerHTML = '';  // Clear previous verbs
            nextButton.classList.add('hidden'); // Hide the next button
            matchedPairs = 0; // Reset matched pairs count
            score = 0;        // Reset score on game initialization
            updateScoreDisplay(); // Update score display

            const shuffledGameData = shuffleArray([...gameData]); // Shuffle game data for emoji containers
            // Create a separate array of verbs and shuffle them independently
            const shuffledVerbs = shuffleArray(gameData.map(data => data.verb));

            // Create and append emoji containers to the images section
            shuffledGameData.forEach(item => {
                const imgContainer = document.createElement('div');
                imgContainer.className = 'image-container group';
                imgContainer.dataset.correctVerb = item.verb;

                const emojiSpan = document.createElement('span');
                emojiSpan.className = 'emoji-display';
                emojiSpan.textContent = item.emoji;
                imgContainer.appendChild(emojiSpan);

                // Create the verb label (will be appended on drag and drop)
                const verbLabel = document.createElement('span');
                verbLabel.className = 'verb-label';

                // Create the div for individual feedback below each emoji container
                const individualFeedback = document.createElement('div');
                individualFeedback.className = 'individual-feedback';
                imgContainer.appendChild(individualFeedback);

                imagesSection.appendChild(imgContainer);

                // Add drag and drop event listeners to emoji containers
                imgContainer.addEventListener('dragover', allowDrop);
                imgContainer.addEventListener('dragleave', handleDragLeave);
                imgContainer.addEventListener('drop', handleDrop);
            });

            // Create and append verb cards to the verbs section
            shuffledVerbs.forEach(verb => {
                const verbCard = document.createElement('div');
                verbCard.className = 'verb-card';
                verbCard.textContent = verb;
                verbCard.draggable = true; // Make the card draggable
                verbCard.dataset.verb = verb; // Store the verb text in a data attribute

                verbsSection.appendChild(verbCard);

                // Add drag event listeners to verb cards
                verbCard.addEventListener('dragstart', handleDragStart);
                verbCard.addEventListener('dragend', handleDragEnd);
            });
        }

        // --- Drag and Drop Handlers ---

        /**
         * Handles the dragstart event for verb cards.
         * Stores the dragged verb element and sets data for the drag operation.
         * @param {DragEvent} event The dragstart event.
         */
        function handleDragStart(event) {
            draggedVerb = event.target; // Reference to the DOM element being dragged
            event.target.classList.add('dragging'); // Add visual feedback for dragging
            // Set the verb text as data for the drag operation
            event.dataTransfer.setData('text/plain', event.target.dataset.verb);
            event.dataTransfer.effectAllowed = 'move'; // Specify allowed drag effect
        }

        /**
         * Handles the dragend event for verb cards.
         * Removes dragging visual feedback.
         * @param {DragEvent} event The dragend event.
         */
        function handleDragEnd(event) {
            event.target.classList.remove('dragging');
            draggedVerb = null; // Clear the dragged verb reference
        }

        /**
         * Handles the dragover event for emoji containers.
         * Prevents default behavior to allow a drop and adds visual feedback.
         * @param {DragEvent} event The dragover event.
         */
        function allowDrop(event) {
            event.preventDefault(); // Necessary to allow a drop
            // Add visual feedback to the drop target
            event.target.closest('.image-container').classList.add('drag-over');
            event.dataTransfer.dropEffect = 'move'; // Indicate a move operation
        }

        /**
         * Handles the dragleave event for emoji containers.
         * Removes visual feedback when the dragged item leaves the drop target.
         * @param {DragEvent} event The dragleave event.
         */
        function handleDragLeave(event) {
            event.target.closest('.image-container').classList.remove('drag-over');
        }

        /**
         * Handles the drop event on emoji containers.
         * Checks if the dropped verb is correct, updates score, and provides feedback.
         * @param {DragEvent} event The drop event.
         */
        function handleDrop(event) {
            event.preventDefault(); // Prevent default browser behavior
            const targetContainer = event.target.closest('.image-container');
            targetContainer.classList.remove('drag-over'); // Remove drag-over styling

            if (!draggedVerb) return; // If no verb is being dragged, do nothing

            const droppedVerbText = draggedVerb.dataset.verb;
            const correctVerb = targetContainer.dataset.correctVerb;
            const individualFeedbackDiv = targetContainer.querySelector('.individual-feedback');

            // Clear previous feedback for this container
            individualFeedbackDiv.textContent = '';
            individualFeedbackDiv.classList.remove('correct', 'incorrect');

            // Remove existing verb if present
            const existingVerbLabel = targetContainer.querySelector('.verb-label');
            if (existingVerbLabel) {
                // If the dragged verb is the same as the already assigned one, do nothing and allow dragging it back if dropped outside
                if (existingVerbLabel.textContent === droppedVerbText) {
                    return;
                }
                // If a new verb is being dropped on a container with an already assigned one,
                // move the existing verb back to the verbs section and then process the new.
                const previouslyAssignedVerb = existingVerbLabel.textContent;
                const originalVerbCard = verbsSection.querySelector(`[data-verb="${previouslyAssignedVerb}"]`);
                if (originalVerbCard) {
                    originalVerbCard.classList.remove('hidden');
                }
                existingVerbLabel.remove();
                targetContainer.classList.remove('has-verb');
                targetContainer.dataset.assignedVerb = ''; // Clear the assigned verb
            }

            // Create a span element to display the dropped verb below the emoji
            const verbLabel = document.createElement('span');
            verbLabel.className = 'verb-label';
            verbLabel.textContent = droppedVerbText;
            targetContainer.appendChild(verbLabel);
            targetContainer.dataset.assignedVerb = droppedVerbText; // Store the assigned verb

            draggedVerb.classList.add('hidden'); // Hide the verb card from the list

            // Check if the dropped verb is correct and provide individual feedback
            if (droppedVerbText === correctVerb) {
                individualFeedbackDiv.textContent = '✔️ Correct';
                individualFeedbackDiv.classList.add('correct');
                targetContainer.classList.add('has-verb'); // Add visual feedback for correct match
                matchedPairs++; // Increment correct matches
                score++; // Increment score for correct answer
            } else {
                individualFeedbackDiv.textContent = '❌ Incorrect';
                individualFeedbackDiv.classList.add('incorrect');
                score--; // Decrement score for incorrect answer
            }
            updateScoreDisplay(); // Update score display after each attempt

            // Check if all pairs are matched
            if (matchedPairs === gameData.length) {
                showMessage("Congratulations! You've completed all matches. You can restart the game.", "success");
                nextButton.classList.remove('hidden'); // Show the next level button
            }
        }

        // --- Message Modal Functions ---

        /**
         * Displays a custom modal message to the user.
         * @param {string} message The message text to display.
         * @param {string} type The type of message (e.g., "info", "success", "error") for styling.
         */
        function showMessage(message, type = "info") {
            modalMessage.textContent = message;
            modalMessage.className = 'py-4 text-lg'; // Reset classes and apply base styling
            if (type === "success") {
                modalMessage.classList.add('text-green-600'); // Tailwind green text
            } else if (type === "error") {
                modalMessage.classList.add('text-red-600'); // Tailwind red text
            } else if (type === "info") {
                modalMessage.classList.add('text-blue-600'); // Tailwind blue text
            }
            messageModal.style.display = 'flex'; // Show the modal using flex for centering
        }

        /**
         * Hides the custom modal message.
         */
        function hideMessage() {
            modalMessage.style.display = 'none'; // Hide the modal
        }

        // Event listeners for the modal close button and outside click
        closeButton.addEventListener('click', hideMessage);
        window.addEventListener('click', (event) => {
            if (event.target == messageModal) {
                hideMessage();
            }
        });

        /**
         * Resets the entire game to its initial state.
         */
        function resetGame() {
            initializeGame();
        }

        // --- Initialize the game when the window loads ---
        window.onload = initializeGame; // Call initializeGame directly as no API generation is needed now

        // Next button functionality (currently just resets the game for a new round)
        nextButton.addEventListener('click', () => {
            initializeGame();
        });

        // Restart button functionality
        restartButton.addEventListener('click', resetGame);
    </script>
</body>
</html>
ELABORADO POR: MARÍA PAZ PERALTA

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Roche Facts Cue Cards</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        :root {
            --custom-blue: #0b41cd;
            --main-background: #ffe8de;
            --card-front-background: #ffffff;
            --card-back-background: #ffffff; /* Changed to white */
            --icon-color: #dbd6d1; /* Updated icon color */
        }
        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--main-background);
        }
        /* -- Card flip animation -- */
        .scene {
            perspective: 1000px;
        }
        .card {
            width: 100%;
            height: 100%;
            position: relative;
            cursor: pointer;
            transform-style: preserve-3d;
            transition: transform 0.8s;
        }
        .card.is-flipped {
            transform: rotateY(180deg);
        }
        .card-face {
            position: absolute;
            width: 100%;
            height: 100%;
            backface-visibility: hidden;
            -webkit-backface-visibility: hidden;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 2rem;
            text-align: center;
            border-radius: 1rem;
            box-shadow: 0 10px 15px -3px rgb(0 0 0 / 0.07), 0 4px 6px -4px rgb(0 0 0 / 0.1);
        }
        .card-face-front {
            background-color: var(--card-front-background);
            color: var(--custom-blue);
            border: 1px solid #e5e7eb;
        }
        .card-face-back {
            background-color: var(--card-back-background);
            color: var(--custom-blue);
            transform: rotateY(180deg);
            border: 1px solid #e5e7eb;
        }
        /* -- Title color override -- */
        .main-title {
            color: var(--custom-blue);
        }
        /* -- Button Styles -- */
        .nav-btn {
            transition: background-color 0.2s, transform 0.2s;
        }
        .nav-btn:hover {
            transform: translateY(-2px);
            background-color: #f0f0f0;
        }
        .filter-btn {
            transition: all 0.2s;
        }
        .filter-btn.active {
            background-color: var(--custom-blue);
            color: white;
            box-shadow: 0 4px 14px 0 rgb(11 65 205 / 20%);
            border-color: var(--custom-blue);
        }
    </style>
</head>
<body class="flex items-center justify-center min-h-screen p-4">

    <div class="w-full max-w-2xl mx-auto">
        <h1 class="text-3xl md:text-4xl font-bold text-center main-title mb-2">Roche History Cue Cards</h1>
        <p class="text-center text-gray-500 mb-8">A journey through key moments and interesting facts.</p>

        <!-- Filter Buttons -->
        <div class="flex justify-center space-x-2 mb-8">
            <button id="filter-all" class="filter-btn py-2 px-4 rounded-full bg-white text-gray-700 font-semibold shadow-sm border border-gray-200">All</button>
            <button id="filter-main" class="filter-btn py-2 px-4 rounded-full bg-white text-gray-700 font-semibold shadow-sm border border-gray-200">Main Facts</button>
            <button id="filter-fun" class="filter-btn py-2 px-4 rounded-full bg-white text-gray-700 font-semibold shadow-sm border border-gray-200">Fun Facts</button>
        </div>

        <!-- Cue Card Component -->
        <div class="scene w-full h-80 md:h-96">
            <div id="card" class="card">
                <div class="card-face card-face-front">
                    <div class="absolute top-4 left-4">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" style="color: var(--icon-color);" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                          <path stroke-linecap="round" stroke-linejoin="round" d="M8.228 9c.549-1.165 2.03-2 3.772-2 2.21 0 4 1.343 4 3 0 1.4-1.278 2.575-3.006 2.907-.542.104-.994.54-.994 1.093m0 3h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                        </svg>
                    </div>
                    <h2 id="card-front-content" class="text-2xl md:text-3xl font-semibold px-8"></h2>
                </div>
                <div class="card-face card-face-back">
                    <div class="absolute top-4 left-4">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" style="color: var(--icon-color);" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                          <path stroke-linecap="round" stroke-linejoin="round" d="M12 8v4m0 4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                        </svg>
                    </div>
                    <p id="card-back-content" class="text-lg md:text-xl"></p>
                </div>
            </div>
        </div>

        <!-- Navigation Controls -->
        <div class="flex items-center justify-between mt-8">
            <button id="prev-btn" class="nav-btn bg-white p-3 rounded-full shadow-md">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-gray-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />
                </svg>
            </button>
            <div id="card-counter" class="text-lg font-semibold text-gray-700"></div>
            <button id="next-btn" class="nav-btn bg-white p-3 rounded-full shadow-md">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-gray-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
                </svg>
            </button>
        </div>
    </div>

    <script>
        // --- Data Source ---
        // Extracted from the provided research document.
        const allCards = [
            {
                category: 'main',
                front: 'Founder\'s Vision',
                back: 'Fritz Hoffmann-La Roche founded the company in 1896, inspired by a cholera outbreak in Hamburg to create industrially manufactured, standardized, and branded medicines.'
            },
            {
                category: 'fun',
                front: 'Company Name Origin',
                back: 'The name "Roche" was not the founder\'s own. It was the maiden name of his wife, Adèle La Roche, adopted to create a distinct and globally recognizable trademark.'
            },
            {
                category: 'main',
                front: 'First Bestseller',
                back: 'The company\'s first major commercial success was Sirolin, an orange-flavored cough syrup launched in 1898. It was so popular it remained on the market for 60 years.'
            },
            {
                category: 'main',
                front: 'The Vitamin Era',
                back: 'In 1934, Roche was the first company to mass-produce synthetic Vitamin C (Redoxon), a breakthrough that transformed public health and made Roche the world\'s leading vitamin supplier.'
            },
            {
                category: 'fun',
                front: 'Serendipitous Discovery',
                back: 'The first benzodiazepine tranquilizer, Librium, was discovered by accident in 1955 when chemist Leo Sternbach decided to test some compounds he had abandoned years earlier during a lab cleanup.'
            },
            {
                category: 'main',
                front: 'The Age of Valium',
                back: 'Launched in 1963, Valium became the most prescribed medication in the world from 1969 to 1982, cementing Roche\'s status as a global pharmaceutical giant.'
            },
            {
                category: 'fun',
                front: 'Accidental Antidepressant',
                back: 'While trying to create a better drug for tuberculosis in 1956, researchers accidentally created iproniazid, which was found to have mood-elevating properties and became the world\'s first antidepressant.'
            },
            {
                category: 'main',
                front: 'Dual-Pillar Strategy',
                back: 'Roche\'s core modern strategy is built on the synergy between its two main divisions: Pharmaceuticals (developing drugs) and Diagnostics (creating tests to guide treatment).'
            },
            {
                category: 'main',
                front: 'The PCR Revolution',
                back: 'In 1991, Roche acquired the rights to Polymerase Chain Reaction (PCR) technology, a foundational tool that allows the amplification of DNA and paved the way for modern molecular diagnostics.'
            },
            {
                category: 'main',
                front: 'Genentech Partnership',
                back: 'Roche acquired a majority stake in biotech pioneer Genentech in 1990 and fully acquired it in 2009, absorbing one of the world\'s most innovative R&D engines, especially in oncology.'
            },
            {
                category: 'fun',
                front: 'A Unique Alliance',
                back: 'When Roche merged with the Japanese company Chugai in 2002, it created a unique "win-win" model where Chugai kept its name, autonomy, and separate stock listing.'
            },
            {
                category: 'main',
                front: 'Oncology Leadership',
                back: 'Through its partnership with Genentech, Roche became a world leader in cancer treatment with blockbuster drugs like Avastin, Herceptin, and Rituxan.'
            },
            {
                category: 'fun',
                front: 'The Vitamin Cartel',
                back: 'Roche was the leader of a decade-long price-fixing cartel for bulk vitamins, resulting in a record-breaking $500 million fine in the U.S. in 1999.'
            },
             {
                category: 'fun',
                front: 'Pioneering Women',
                back: 'In 1925, Roche appointed Alice Keller as the General Manager of its Tokyo subsidiary, a sensational achievement for a woman in that era.'
            },
             {
                category: 'main',
                front: 'Diagnostics Dominance',
                back: 'With the $11 billion acquisition of Boehringer Mannheim in 1998, Roche instantly became the world leader in the in-vitro diagnostics market.'
            },
            {
                category: 'main',
                front: 'Parkinson\'s Treatment',
                back: 'In 1973, Roche launched Madopar, a combination therapy for Parkinson\'s disease that became a significant advance in treating the condition.'
            },
            {
                category: 'main',
                front: 'The Seveso Disaster',
                back: 'In 1976, a chemical plant owned by a Roche subsidiary in Seveso, Italy, released a toxic cloud of dioxin, causing a major environmental and public relations crisis.'
            },
            {
                category: 'main',
                front: 'Tissue Diagnostics',
                back: 'Roche bolstered its diagnostics leadership by acquiring Ventana Medical Systems in 2008, a leader in automated tissue-based cancer diagnostics.'
            },
            {
                category: 'main',
                front: 'Data-Driven Research',
                back: 'In 2018, Roche acquired Flatiron Health, gaining access to high-quality, real-world oncology data from electronic health records to accelerate cancer research.'
            },
            {
                category: 'main',
                front: 'Gene Therapy Entry',
                back: 'Roche entered the high-growth field of gene therapy with its 2019 acquisition of Spark Therapeutics, the company behind an approved treatment for a rare form of blindness.'
            }
        ];

        // --- App State ---
        let currentCards = [...allCards];
        let currentIndex = 0;
        let activeFilter = 'all';

        // --- DOM Elements ---
        const card = document.getElementById('card');
        const cardFront = document.getElementById('card-front-content');
        const cardBack = document.getElementById('card-back-content');
        const cardCounter = document.getElementById('card-counter');
        const prevBtn = document.getElementById('prev-btn');
        const nextBtn = document.getElementById('next-btn');
        const filterBtns = {
            all: document.getElementById('filter-all'),
            main: document.getElementById('filter-main'),
            fun: document.getElementById('filter-fun')
        };

        // --- Functions ---

        /**
         * Updates the card content and counter display.
         */
        function displayCard() {
            if (currentCards.length === 0) {
                cardFront.textContent = 'No cards to show';
                cardBack.textContent = 'Please select another category.';
                cardCounter.textContent = '0 / 0';
                return;
            }

            const currentCardData = currentCards[currentIndex];
            cardFront.textContent = currentCardData.front;
            cardBack.textContent = currentCardData.back;
            cardCounter.textContent = `${currentIndex + 1} / ${currentCards.length}`;
            
            // Ensure card is not flipped when content changes
            if (card.classList.contains('is-flipped')) {
                card.classList.remove('is-flipped');
            }
        }

        /**
         * Flips the card.
         */
        function flipCard() {
            card.classList.toggle('is-flipped');
        }

        /**
         * Navigates to the next card.
         */
        function nextCard() {
            if (currentCards.length === 0) return;
            currentIndex = (currentIndex + 1) % currentCards.length;
            displayCard();
        }

        /**
         * Navigates to the previous card.
         */
        function prevCard() {
            if (currentCards.length === 0) return;
            currentIndex = (currentIndex - 1 + currentCards.length) % currentCards.length;
            displayCard();
        }
        
        /**
         * Filters the cards by category and updates the view.
         * @param {string} category - The category to filter by ('all', 'main', 'fun').
         */
        function filterCards(category) {
            activeFilter = category;
            
            if (category === 'all') {
                currentCards = [...allCards];
            } else {
                currentCards = allCards.filter(c => c.category === category);
            }
            
            currentIndex = 0;
            updateFilterButtons();
            displayCard();
        }

        /**
         * Updates the visual state of the filter buttons.
         */
        function updateFilterButtons() {
            for (const key in filterBtns) {
                if (key === activeFilter) {
                    filterBtns[key].classList.add('active');
                } else {
                    filterBtns[key].classList.remove('active');
                }
            }
        }

        // --- Event Listeners ---
        card.addEventListener('click', flipCard);
        nextBtn.addEventListener('click', nextCard);
        prevBtn.addEventListener('click', prevCard);
        
        filterBtns.all.addEventListener('click', () => filterCards('all'));
        filterBtns.main.addEventListener('click', () => filterCards('main'));
        filterBtns.fun.addEventListener('click', () => filterCards('fun'));

        // --- Initial Load ---
        filterCards('all'); // Start with all cards shown

    </script>
</body>
</html>

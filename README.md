<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MovieFlix - User Panel</title>
    <style>
        :root {
            --primary-color: #141414;
            --secondary-color: #2e2e2e;
            --text-color: #ffffff;
            --accent-color: #e50914;
        }
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            background-color: var(--primary-color);
            color: var(--text-color);
        }
        header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 1rem 2rem;
            background-color: rgba(20, 20, 20, 0.8);
            backdrop-filter: blur(10px);
            position: sticky;
            top: 0;
            z-index: 100;
        }
        .logo {
            font-size: 1.8rem;
            font-weight: bold;
            color: var(--accent-color);
        }
        .search-container {
            position: relative;
        }
        #search-input {
            padding: 0.5rem 1rem;
            border-radius: 20px;
            border: 1px solid var(--secondary-color);
            background-color: var(--secondary-color);
            color: var(--text-color);
            width: 250px;
            transition: width 0.3s ease;
        }
        #search-input:focus {
            outline: none;
            width: 300px;
        }
        .container {
            padding: 1.5rem; /* पैडिंग को थोड़ा कम किया */
        }
        .section-title {
            font-size: 1.5rem;
            margin-bottom: 1rem;
            border-left: 4px solid var(--accent-color);
            padding-left: 10px;
        }
        .grid {
            display: grid;
            /* बदला हुआ: ग्रिड को हमेशा 2 कॉलम में सेट किया गया है */
            grid-template-columns: repeat(2, 1fr);
            /* बदला हुआ: कार्ड के बीच की जगह */
            gap: 1rem;
        }
        .card {
            background-color: var(--secondary-color);
            border-radius: 8px;
            overflow: hidden;
            cursor: pointer;
            transition: transform 0.2s ease, box-shadow 0.2s ease;
            position: relative;
        }
        .card:hover {
            transform: scale(1.05);
            box-shadow: 0 8px 16px rgba(0,0,0,0.5);
        }
        .card img {
            width: 100%;
            /* बदला हुआ: पोस्टर की ऊंचाई बड़े कार्ड के लिए एडजस्ट की गई है */
            height: 250px;
            object-fit: cover;
        }
        .card-title {
            padding: 0.6rem 0.8rem;
            font-size: 0.9rem; /* फॉन्ट साइज थोड़ा बड़ा किया गया */
            font-weight: bold;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }
        #detail-view {
            display: none;
            padding: 2rem;
            animation: fadeIn 0.5s;
        }
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        .detail-header {
            display: flex;
            gap: 2rem;
            flex-wrap: wrap;
        }
        .detail-poster img {
            width: 250px;
            border-radius: 8px;
        }
        .detail-info {
            flex: 1;
        }
        .detail-info h1 {
            font-size: 2.5rem;
            margin-top: 0;
            color: var(--accent-color);
        }
        .detail-info p {
            line-height: 1.6;
        }
        .info-item {
            margin: 10px 0;
        }
        .info-item span {
            font-weight: bold;
            color: #aaa;
        }
        .btn {
            padding: 0.8rem 1.5rem;
            border: none;
            border-radius: 5px;
            background-color: var(--accent-color);
            color: var(--text-color);
            font-size: 1rem;
            cursor: pointer;
            text-decoration: none;
            margin-right: 10px;
            transition: background-color 0.2s ease;
        }
        .btn:hover {
            background-color: #c40812;
        }
        .download-links, #episodes-list {
            margin-top: 2rem;
        }
        .episode-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 1rem;
            background-color: var(--secondary-color);
            border-radius: 5px;
            margin-bottom: 10px;
        }
        #back-button {
            margin-bottom: 2rem;
        }

        /* बड़ी स्क्रीन (जैसे टैबलेट और डेस्कटॉप) के लिए मीडिया क्वेरी */
        @media (min-width: 768px) {
            .grid {
                /* बड़ी स्क्रीन पर एक लाइन में 4 या अधिक पोस्ट दिखाएं */
                grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
            }
            .container {
                padding: 2rem; /* बड़ी स्क्रीन पर पैडिंग वापस लाएं */
            }
        }
    </style>
</head>
<body>

    <header>
        <div class="logo">MovieFlix</div>
        <div class="search-container">
            <input type="text" id="search-input" placeholder="Search for movies, anime, series...">
        </div>
    </header>

    <main id="main-view" class="container">
        <section id="new-releases">
            <h2 class="section-title">Newly Added</h2>
            <div id="new-releases-grid" class="grid"></div>
        </section>
        <section id="all-content" style="margin-top: 3rem;">
            <h2 class="section-title">Explore More</h2>
            <div id="all-content-grid" class="grid"></div>
        </section>
    </main>
    
    <div id="detail-view">
        <button id="back-button" class="btn">← Back to List</button>
        <div class="detail-header">
            <div class="detail-poster">
                <img id="detail-poster-img" src="" alt="Poster">
            </div>
            <div class="detail-info">
                <h1 id="detail-title"></h1>
                <p id="detail-description"></p>
                <div class="info-item"><span>Genre:</span> <span id="detail-genre"></span></div>
                <div class="info-item" id="verdict-container"><span>Verdict:</span> <span id="detail-verdict"></span></div>
                <a id="detail-trailer-link" href="#" target="_blank" class="btn">View Trailer</a>
            </div>
        </div>

        <div style="margin: 2rem 0; text-align: center;">
            <script async="async" data-cfasync="false" src="//pl27363002.profitableratecpm.com/dc83d58651e13cd7224dd5a3471b1a21/invoke.js"></script>
            <div id="container-dc83d58651e13cd7224dd5a3471b1a21"></div>
        </div>

        <div id="movie-downloads" class="download-links">
            <h3 class="section-title">Download Movie</h3>
            <a id="download-480p" href="#" class="btn" download>480p</a>
            <a id="download-720p" href="#" class="btn" download>720p</a>
            <a id="download-1080p" href="#" class="btn" download>1080p</a>
        </div>
        
        <div id="series-downloads">
            <h3 class="section-title">Episodes</h3>
            <div id="episodes-list"></div>
        </div>
    </div>

    <!-- Firebase SDK (कोई बदलाव नहीं) -->
    <script type="module">
        // Import functions from the SDKs
        import { initializeApp } from "https://www.gstatic.com/firebasejs/9.15.0/firebase-app.js";
        import { getFirestore, collection, getDocs, doc, getDoc } from "https://www.gstatic.com/firebasejs/9.15.0/firebase-firestore.js";

        // !! जरूरी: अपनी Firebase कॉन्फ़िगरेशन यहां पेस्ट करें !!
        const firebaseConfig = {
            apiKey: "AIzaSyAjj_AwiX9Ao8Vbz_8-ULixU0NBfnK2G7A",
  authDomain: "digi-admin-f0209.firebaseapp.com",
  databaseURL: "https://digi-admin-f0209-default-rtdb.firebaseio.com",
  projectId: "digi-admin-f0209",
  storageBucket: "digi-admin-f0209.firebasestorage.app",
  messagingSenderId: "978010257926",
  appId: "1:978010257926:web:0b49988d4658e405e76187"
        };

        // Initialize Firebase
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        const mainView = document.getElementById('main-view');
        const detailView = document.getElementById('detail-view');
        const newReleasesGrid = document.getElementById('new-releases-grid');
        const allContentGrid = document.getElementById('all-content-grid');
        const searchInput = document.getElementById('search-input');
        
        let allContentData = [];

        async function loadContent() {
            const querySnapshot = await getDocs(collection(db, "content"));
            allContentData = [];
            querySnapshot.forEach((doc) => {
                allContentData.push({ id: doc.id, ...doc.data() });
            });
            displayContent(allContentData);
        }

        function displayContent(data) {
            newReleasesGrid.innerHTML = '';
            allContentGrid.innerHTML = '';
            
            data.sort((a, b) => (b.timestamp?.toMillis() || 0) - (a.timestamp?.toMillis() || 0));

            const newReleases = data.slice(0, 5); // You can adjust this number
            const allContent = data;

            newReleases.forEach(item => createCard(item, newReleasesGrid));
            allContent.forEach(item => createCard(item, allContentGrid));
        }

        function createCard(item, gridElement) {
            const card = document.createElement('div');
            card.className = 'card';
            card.dataset.id = item.id;
            card.innerHTML = `
                <img src="${item.posterUrl}" alt="${item.title}">
                <div class="card-title">${item.title}</div>
            `;
            card.addEventListener('click', () => showDetails(item.id));
            gridElement.appendChild(card);
        }
        
        async function showDetails(id) {
            mainView.style.display = 'none';
            detailView.style.display = 'block';
            window.scrollTo(0, 0);

            const docRef = doc(db, "content", id);
            const docSnap = await getDoc(docRef);

            if (docSnap.exists()) {
                const data = docSnap.data();
                document.getElementById('detail-poster-img').src = data.posterUrl;
                document.getElementById('detail-title').textContent = data.title;
                document.getElementById('detail-description').textContent = data.description;
                document.getElementById('detail-genre').textContent = data.genre;
                document.getElementById('detail-trailer-link').href = data.trailerUrl;
                
                if (data.type === 'movie') {
                    document.getElementById('movie-downloads').style.display = 'block';
                    document.getElementById('series-downloads').style.display = 'none';
                    document.getElementById('verdict-container').style.display = 'block';

                    document.getElementById('detail-verdict').textContent = data.verdict;
                    document.getElementById('download-480p').href = data.downloads['480p'] || '#';
                    document.getElementById('download-720p').href = data.downloads['720p'] || '#';
                    document.getElementById('download-1080p').href = data.downloads['1080p'] || '#';
                } else if (data.type === 'series') {
                    document.getElementById('movie-downloads').style.display = 'none';
                    document.getElementById('series-downloads').style.display = 'block';
                    document.getElementById('verdict-container').style.display = 'none';

                    const episodesList = document.getElementById('episodes-list');
                    episodesList.innerHTML = '';
                    data.episodes.forEach((ep, index) => {
                        const episodeItem = document.createElement('div');
                        episodeItem.className = 'episode-item';
                        episodeItem.innerHTML = `
                            <span>Episode ${index + 1}</span>
                            <div>
                                <a href="${ep['720p']}" class="btn" download>720p</a>
                                <a href="${ep['1080p']}" class="btn" download>1080p</a>
                            </div>
                        `;
                        episodesList.appendChild(episodeItem);
                    });
                }
            } else {
                console.log("No such document!");
            }
        }
        
        document.getElementById('back-button').addEventListener('click', () => {
            detailView.style.display = 'none';
            mainView.style.display = 'block';
        });

        searchInput.addEventListener('input', (e) => {
            const searchTerm = e.target.value.toLowerCase();
            const filteredData = allContentData.filter(item => 
                item.title.toLowerCase().includes(searchTerm)
            );
            displayContent(filteredData);
        });

        loadContent();
    </script>
</body><script type='text/javascript' src='//pl27363028.profitableratecpm.com/12/1b/79/121b79b3f38aadbe2108323d0999e7ac.js'></script></body>
</body>
</html>

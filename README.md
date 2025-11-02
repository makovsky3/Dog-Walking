<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PawsConnect - Find Your Perfect Dog Walker</title>
    <!-- Load Tailwind CSS for styling -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Custom styles and font setup */
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@100..900&display=swap');
        :root {
            --primary-color: #3b82f6; /* Tailwind blue-500 */
            --secondary-color: #f59e0b; /* Tailwind amber-500 */
            --bg-color: #f3f4f6; /* Tailwind gray-100 */
            --card-bg: #ffffff;
        }
        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg-color);
            transition: background-color 0.3s;
        }
        /* Custom scrollbar for better aesthetics */
        .listing-container::-webkit-scrollbar { width: 8px; }
        .listing-container::-webkit-scrollbar-thumb {
            background-color: var(--secondary-color);
            border-radius: 4px;
        }
        .listing-container::-webkit-scrollbar-track {
            background-color: var(--card-bg);
        }
        /* Utility for button focus/hover */
        .btn-primary {
            transition: all 0.2s;
        }
        .btn-primary:hover {
            transform: translateY(-1px);
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -2px rgba(0, 0, 0, 0.06);
        }
    </style>
</head>
<body class="min-h-screen flex flex-col">

    <!-- Header Section -->
    <header class="bg-white shadow-lg sticky top-0 z-10">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4 flex justify-between items-center">
            <h1 class="text-3xl font-extrabold text-gray-900 flex items-center">
                <span class="text-3xl mr-2 text-yellow-600">🐾</span> PawsConnect
            </h1>
            <div class="flex items-center space-x-4">
                <div id="auth-status" class="text-sm font-medium text-gray-600">
                    <span id="user-display">Connecting...</span>
                </div>
                <!-- Button to open the posting modal -->
                <button id="open-modal-btn" class="btn-primary bg-blue-600 text-white px-4 py-2 rounded-lg font-semibold hover:bg-blue-700 focus:outline-none focus:ring-4 focus:ring-blue-500 focus:ring-opacity-50 shadow-md">
                    + New Post
                </button>
            </div>
        </div>
    </header>

    <!-- Main Content Area -->
    <main class="flex-grow max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8 w-full">
        <h2 class="text-4xl font-bold text-gray-800 mb-6 text-center">
            Find the perfect companion or the perfect service.
        </h2>

        <!-- Tab Controls for Listing Type -->
        <div class="flex justify-center mb-8 space-x-4">
            <button id="tab-walkers" data-type="Walker" class="tab-btn px-6 py-3 text-lg font-semibold rounded-lg shadow-md transition-colors duration-200 bg-blue-500 text-white">
                Dog Walkers
            </button>
            <button id="tab-owners" data-type="Owner" class="tab-btn px-6 py-3 text-lg font-semibold rounded-lg shadow-md transition-colors duration-200 bg-gray-200 text-gray-700 hover:bg-gray-300">
                Owners Needing Help
            </button>
        </div>

        <!-- Listing Display Area -->
        <div class="listing-container grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 auto-rows-min">
            <!-- Listings will be rendered here by JavaScript -->
            <div id="listings-placeholder" class="lg:col-span-3 text-center p-10 text-gray-500 bg-white rounded-xl shadow-lg">
                <svg class="mx-auto h-12 w-12 text-gray-400" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4.5v15m7.5-7.5h-15m7.5 0l-7.5-7.5m7.5 7.5l7.5-7.5"/>
                </svg>
                <h3 class="mt-2 text-sm font-medium text-gray-900">No Listings Yet</h3>
                <p class="mt-1 text-sm text-gray-500">
                    Be the first to post a listing or a request!
                </p>
            </div>
            <div id="listings-loading" class="hidden lg:col-span-3 text-center p-10 text-blue-500 bg-white rounded-xl shadow-lg">
                <svg class="animate-spin -ml-1 mr-3 h-5 w-5 text-blue-500 inline-block" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                    <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                    <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
                </svg>
                Loading listings...
            </div>
        </div>
    </main>

    <!-- Footer Section -->
    <footer class="bg-white shadow-inner mt-10">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4 text-center text-gray-500 text-sm">
            &copy; <span id="current-year"></span> PawsConnect - Backend powered by Firebase Firestore.
        </div>
    </footer>

    <!-- Modal for New Post Form -->
    <div id="post-modal" class="hidden fixed inset-0 z-50 bg-gray-900 bg-opacity-75 flex items-center justify-center p-4 transition-opacity duration-300">
        <div class="bg-white rounded-xl p-8 max-w-lg w-full shadow-2xl transform scale-100 transition-transform duration-300">
            <div class="flex justify-between items-center mb-6">
                <h3 class="text-2xl font-bold text-gray-800" id="modal-title">Create New Walker Listing</h3>
                <button id="close-modal-btn" class="text-gray-400 hover:text-gray-600 focus:outline-none">
                    <svg class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/>
                    </svg>
                </button>
            </div>

            <form id="post-form" class="space-y-4">
                <!-- Post Type Selection -->
                <div>
                    <label for="post-type" class="block text-sm font-medium text-gray-700">What are you posting?</label>
                    <select id="post-type" name="type" required class="mt-1 block w-full pl-3 pr-10 py-2 text-base border-gray-300 focus:outline-none focus:ring-blue-500 focus:border-blue-500 sm:text-sm rounded-md shadow-sm transition duration-150 ease-in-out">
                        <option value="Walker" selected>Walker: Offering Services (I walk dogs)</option>
                        <option value="Owner">Owner: Requesting Services (I need a walker)</option>
                    </select>
                </div>

                <!-- Title Input -->
                <div>
                    <label for="post-title" class="block text-sm font-medium text-gray-700">Title / Short Summary (e.g., 'Experienced Walker in Downtown' or 'Need Weekend Walks')</label>
                    <input type="text" id="post-title" name="title" required maxlength="100" class="mt-1 block w-full border border-gray-300 rounded-md shadow-sm py-2 px-3 focus:outline-none focus:ring-blue-500 focus:border-blue-500 transition duration-150 ease-in-out">
                </div>

                <!-- Description Input -->
                <div>
                    <label for="post-description" class="block text-sm font-medium text-gray-700">Details</label>
                    <textarea id="post-description" name="description" rows="3" required maxlength="500" class="mt-1 block w-full border border-gray-300 rounded-md shadow-sm py-2 px-3 focus:outline-none focus:ring-blue-500 focus:border-blue-500 transition duration-150 ease-in-out"></textarea>
                </div>

                <!-- Zip Code Input -->
                <div>
                    <label for="post-zip-code" class="block text-sm font-medium text-gray-700">Service Area Zip Code</label>
                    <input type="text" id="post-zip-code" name="zipCode" required pattern="\d{5}" maxlength="5" class="mt-1 block w-full border border-gray-300 rounded-md shadow-sm py-2 px-3 focus:outline-none focus:ring-blue-500 focus:border-blue-500 transition duration-150 ease-in-out" placeholder="e.g., 90210">
                </div>

                <!-- Submit Button and Status -->
                <div class="flex items-center justify-between pt-4">
                    <button type="submit" id="submit-post-btn" class="btn-primary w-full bg-yellow-500 text-white px-4 py-2 rounded-lg font-semibold hover:bg-yellow-600 focus:outline-none focus:ring-4 focus:ring-yellow-500 focus:ring-opacity-50 shadow-md">
                        Publish Post
                    </button>
                </div>
                <div id="form-message" class="text-sm text-center font-medium mt-2 hidden"></div>
            </form>
        </div>
    </div>

    <!-- Message Box/Toast Notification Area -->
    <div id="message-container" class="fixed bottom-4 right-4 space-y-2 z-50">
        <!-- Messages will be injected here -->
    </div>

    <!-- Firebase Initialization and Application Logic -->
    <script type="module">
        // Import necessary Firebase modules
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, addDoc, onSnapshot, collection, query, where, serverTimestamp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
        import { setLogLevel } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore-lite.js";

        // Global Firebase variables provided by the canvas environment
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        const firebaseConfig = JSON.parse(typeof __firebase_config !== 'undefined' ? __firebase_config : '{}');
        const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;

        // Application State
        let db, auth;
        let userId = null;
        let userName = 'Guest User';
        let currentListingType = 'Walker'; // Default view
        let unsubscribe = null; // To hold the real-time listener

        // --- Utility Functions ---

        /**
         * Shows a temporary, styled message/toast notification.
         * @param {string} message The content of the message.
         * @param {string} type 'success', 'error', or 'info'.
         */
        const showMessage = (message, type = 'info') => {
            const container = document.getElementById('message-container');
            const colorMap = {
                'success': 'bg-green-500',
                'error': 'bg-red-500',
                'info': 'bg-blue-500'
            };

            const toast = document.createElement('div');
            toast.className = `p-4 rounded-lg shadow-xl text-white font-semibold transition-all duration-300 transform translate-x-0 ${colorMap[type] || colorMap['info']}`;
            toast.textContent = message;

            container.appendChild(toast);

            // Automatically hide after 4 seconds
            setTimeout(() => {
                toast.classList.add('opacity-0');
                toast.addEventListener('transitionend', () => toast.remove());
            }, 4000);
        };

        /**
         * Formats a Firebase Timestamp object into a readable date string.
         * @param {object} timestamp The Firebase Timestamp object.
         * @returns {string} Formatted date string.
         */
        const formatDate = (timestamp) => {
            if (!timestamp || !timestamp.toDate) return 'Just now';
            const date = timestamp.toDate();
            return date.toLocaleDateString('en-US', {
                year: 'numeric',
                month: 'short',
                day: 'numeric',
                hour: '2-digit',
                minute: '2-digit'
            });
        };

        /**
         * Gets the Firestore collection path for public listings.
         * @returns {string} The public collection path.
         */
        const getPublicCollectionPath = () => {
            return `/artifacts/${appId}/public/data/dog_walker_listings`;
        };

        // --- UI Rendering Functions ---

        /**
         * Renders a single listing card.
         * @param {object} listing The listing data object.
         * @returns {string} The HTML string for the card.
         */
        const renderListingCard = (listing) => {
            const isOwner = listing.type === 'Owner';
            const colorClass = isOwner ? 'bg-amber-100 border-amber-500' : 'bg-blue-100 border-blue-500';
            const badgeText = isOwner ? 'Service Needed' : 'Service Offered';
            const icon = isOwner ? '🐾' : '🐶';

            return `
                <div class="p-6 ${colorClass} border-l-4 rounded-xl shadow-lg hover:shadow-xl transition-shadow duration-300">
                    <div class="flex items-center justify-between mb-3">
                        <span class="text-xs font-bold uppercase tracking-wider text-white px-3 py-1 rounded-full ${isOwner ? 'bg-amber-500' : 'bg-blue-500'} shadow-sm">
                            ${badgeText}
                        </span>
                        ${listing.userId === userId ? '<span class="text-xs text-red-500 font-bold">YOUR POST</span>' : ''}
                    </div>
                    <h4 class="text-xl font-bold text-gray-900 mb-2">${icon} ${listing.title}</h4>
                    <p class="text-gray-600 mb-4 text-sm">${listing.description}</p>
                    <div class="space-y-1 text-sm text-gray-700">
                        <p><span class="font-semibold text-gray-900">User:</span> ${listing.userName}</p>
                        <p><span class="font-semibold text-gray-900">Zip Code:</span> <span class="text-lg font-mono text-gray-800">${listing.zipCode}</span></p>
                        <p><span class="font-semibold text-gray-900">Posted:</span> ${formatDate(listing.timestamp)}</p>
                    </div>
                    <button class="mt-4 w-full bg-green-500 text-white py-2 rounded-lg text-sm font-semibold hover:bg-green-600 transition-colors">
                        Contact ${isOwner ? 'Owner' : 'Walker'} (Not implemented)
                    </button>
                </div>
            `;
        };

        /**
         * Fetches and displays listings based on the current listing type (Walker/Owner).
         */
        const loadListings = () => {
            const container = document.querySelector('.listing-container');
            const loading = document.getElementById('listings-loading');
            const placeholder = document.getElementById('listings-placeholder');

            // Show loading state and clear previous listings
            container.innerHTML = '';
            placeholder.classList.add('hidden');
            loading.classList.remove('hidden');

            // 1. Unsubscribe from previous listener if one exists
            if (unsubscribe) {
                unsubscribe();
                unsubscribe = null;
            }

            // 2. Define the Firestore query
            const listingsRef = collection(db, getPublicCollectionPath());
            const listingsQuery = query(
                listingsRef,
                where('type', '==', currentListingType)
                // Note: We avoid orderBy here to prevent index creation requirements in the sandbox environment.
            );

            // 3. Set up the real-time listener (onSnapshot)
            unsubscribe = onSnapshot(listingsQuery, (snapshot) => {
                const listings = [];
                snapshot.forEach(doc => {
                    listings.push({ id: doc.id, ...doc.data() });
                });

                // Sort the data client-side by timestamp in descending order (newest first)
                listings.sort((a, b) => {
                    const timeA = a.timestamp?.toDate ? a.timestamp.toDate().getTime() : 0;
                    const timeB = b.timestamp?.toDate ? b.timestamp.toDate().getTime() : 0;
                    return timeB - timeA;
                });

                // Render the listings
                container.innerHTML = ''; // Clear container again for updates
                loading.classList.add('hidden');

                if (listings.length === 0) {
                    placeholder.classList.remove('hidden');
                } else {
                    listings.forEach(listing => {
                        container.innerHTML += renderListingCard(listing);
                    });
                }
                
                showMessage(`Updated: Showing ${listings.length} ${currentListingType} posts.`, 'info');

            }, (error) => {
                console.error("Error listening to Firestore:", error);
                loading.classList.add('hidden');
                container.innerHTML = `<div class="lg:col-span-3 p-10 bg-red-100 text-red-700 rounded-xl shadow-lg">Error loading listings. Please check console for details.</div>`;
                showMessage('Failed to load real-time listings.', 'error');
            });
        };

        // --- Firestore Interaction Functions ---

        /**
         * Handles the submission of a new post to Firestore.
         * @param {Event} e The submit event.
         */
        const handlePostSubmit = async (e) => {
            e.preventDefault();
            const form = e.target;
            const submitBtn = document.getElementById('submit-post-btn');
            const messageDiv = document.getElementById('form-message');

            messageDiv.textContent = 'Publishing...';
            messageDiv.classList.remove('hidden', 'text-red-500', 'text-green-500');
            messageDiv.classList.add('text-blue-500');
            submitBtn.disabled = true;

            const formData = new FormData(form);
            const postData = {
                type: formData.get('type'), // 'Walker' or 'Owner'
                title: formData.get('title').trim(),
                description: formData.get('description').trim(),
                zipCode: formData.get('zipCode').trim(),
                userId: userId,
                userName: userName,
                timestamp: serverTimestamp() // Firestore's server-generated timestamp
            };

            // Basic validation
            if (!postData.title || !postData.description || postData.zipCode.length !== 5) {
                messageDiv.textContent = 'Please fill out all fields correctly.';
                messageDiv.classList.replace('text-blue-500', 'text-red-500');
                submitBtn.disabled = false;
                return;
            }

            try {
                // Add a new document to the public collection
                const docRef = await addDoc(collection(db, getPublicCollectionPath()), postData);

                messageDiv.textContent = 'Post published successfully!';
                messageDiv.classList.replace('text-blue-500', 'text-green-500');
                showMessage(`New post added with ID: ${docRef.id}`, 'success');

                // Clear form and close modal after a short delay
                form.reset();
                setTimeout(() => {
                    document.getElementById('post-modal').classList.add('hidden');
                }, 1000);

            } catch (error) {
                console.error("Error adding document: ", error);
                messageDiv.textContent = `Error publishing post: ${error.message}`;
                messageDiv.classList.replace('text-blue-500', 'text-red-500');
                showMessage('Failed to publish post.', 'error');
            } finally {
                submitBtn.disabled = false;
            }
        };

        // --- Initialization and Event Handlers ---

        /**
         * Initializes Firebase and handles the authentication process.
         */
        const initializeAppAndAuth = async () => {
            try {
                // 1. Initialize Firebase App and Services
                const app = initializeApp(firebaseConfig);
                db = getFirestore(app);
                auth = getAuth(app);
                setLogLevel('debug'); // Enable detailed logging for Firestore

                // 2. Handle Authentication
                // onAuthStateChanged will handle the token or anonymous sign-in logic
                onAuthStateChanged(auth, async (user) => {
                    if (user) {
                        // User is signed in.
                        userId = user.uid;
                        userName = `User-${userId.substring(0, 4)}`; // Simple display name
                        document.getElementById('user-display').textContent = `Authenticated as: ${userName}`;

                        // Now that we have the userId, we can load the data
                        loadListings();
                    } else {
                        // User is signed out. Sign in anonymously or with custom token.
                        try {
                            if (initialAuthToken) {
                                // Use the provided custom token for sign-in
                                await signInWithCustomToken(auth, initialAuthToken);
                            } else {
                                // Fallback to anonymous sign-in
                                await signInAnonymously(auth);
                            }
                            // onAuthStateChanged will be called again on success
                        } catch (error) {
                            console.error("Authentication Error:", error);
                            document.getElementById('user-display').textContent = 'Auth Failed';
                            showMessage(`Authentication failed: ${error.message}.`, 'error');
                        }
                    }
                });

            } catch (e) {
                console.error("Firebase Initialization Error:", e);
                document.getElementById('user-display').textContent = 'Initialization Failed';
                showMessage('Firebase initialization failed. Check your configuration.', 'error');
            }
        };

        /**
         * Sets up all necessary DOM event listeners.
         */
        const setupEventListeners = () => {
            // Modal controls
            const modal = document.getElementById('post-modal');
            document.getElementById('open-modal-btn').addEventListener('click', () => {
                modal.classList.remove('hidden');
            });
            document.getElementById('close-modal-btn').addEventListener('click', () => {
                modal.classList.add('hidden');
            });
            // Close modal when clicking outside
            modal.addEventListener('click', (e) => {
                if (e.target === modal) {
                    modal.classList.add('hidden');
                }
            });

            // Form submission
            document.getElementById('post-form').addEventListener('submit', handlePostSubmit);

            // Post Type change listener (updates modal title)
            document.getElementById('post-type').addEventListener('change', (e) => {
                const type = e.target.value;
                const titleElement = document.getElementById('modal-title');
                if (type === 'Walker') {
                    titleElement.textContent = 'Create New Walker Listing';
                } else {
                    titleElement.textContent = 'Create New Owner Request';
                }
            });

            // Tab switching
            document.querySelectorAll('.tab-btn').forEach(button => {
                button.addEventListener('click', (e) => {
                    const newType = e.target.dataset.type;

                    if (newType !== currentListingType) {
                        currentListingType = newType;

                        // Update button styling
                        document.querySelectorAll('.tab-btn').forEach(btn => {
                            if (btn.dataset.type === currentListingType) {
                                btn.classList.replace('bg-gray-200', 'bg-blue-500');
                                btn.classList.replace('text-gray-700', 'text-white');
                                btn.classList.remove('hover:bg-gray-300');
                            } else {
                                btn.classList.replace('bg-blue-500', 'bg-gray-200');
                                btn.classList.replace('text-white', 'text-gray-700');
                                btn.classList.add('hover:bg-gray-300');
                            }
                        });

                        // Reload listings with the new filter
                        if (db) {
                            loadListings();
                        }
                    }
                });
            });
        };

        // --- Application Bootstrap ---

        window.onload = () => {
            // Set current year in footer
            document.getElementById('current-year').textContent = new Date().getFullYear();

            // Setup listeners for UI interactions
            setupEventListeners();

            // Initialize Firebase and start authentication flow
            initializeAppAndAuth();
        };

    </script>
</body>
</html>

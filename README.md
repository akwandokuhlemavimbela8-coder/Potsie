# Potsie
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SocialSync - Video Distribution Platform</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: { sans: ['Inter', 'sans-serif'] },
                    colors: {
                        brand: { 50: '#f0f9ff', 100: '#e0f2fe', 500: '#0ea5e9', 600: '#0284c7', 900: '#0c4a6e' },
                        instagram: '#E4405F', youtube: '#FF0000', tiktok: '#000000', twitter: '#1DA1F2', facebook: '#1877F2'
                    }
                }
            }
        }
    </script>
    <style>
        .glass { background: rgba(255,255,255,0.95); backdrop-filter: blur(10px); }
        .drag-active { border-color: #0ea5e9; background: #f0f9ff; transform: scale(1.02); }
        .platform-card { transition: all 0.3s ease; }
        .platform-card:hover { transform: translateY(-2px); box-shadow: 0 10px 25px -5px rgba(0,0,0,0.1); }
        .platform-card.selected { border-color: #0ea5e9; background: #f0f9ff; }
        .video-card { transition: all 0.3s ease; }
        .video-card:hover { transform: translateY(-2px); box-shadow: 0 10px 25px -5px rgba(0,0,0,0.1); }
        .timeline-item { position: relative; padding-left: 2rem; }
        .timeline-item::before { content: ''; position: absolute; left: 0.5rem; top: 0; bottom: 0; width: 2px; background: #e5e7eb; }
        .timeline-item::after { content: ''; position: absolute; left: 0.25rem; top: 0.5rem; width: 0.75rem; height: 0.75rem; border-radius: 50%; background: #0ea5e9; }
        .timeline-item:last-child::before { display: none; }
        @keyframes pulse-ring { 0% { transform: scale(0.8); opacity: 1; } 100% { transform: scale(1.3); opacity: 0; } }
        .status-dot { position: relative; }
        .status-dot::after { content: ''; position: absolute; inset: -4px; border-radius: 50%; border: 2px solid currentColor; animation: pulse-ring 2s infinite; }
        .scrollbar-hide::-webkit-scrollbar { display: none; }
        .scrollbar-hide { -ms-overflow-style: none; scrollbar-width: none; }
    </style>
</head>
<body class="bg-gray-50 text-gray-800 font-sans antialiased">

    <!-- Navigation -->
    <nav class="glass sticky top-0 z-50 border-b border-gray-200">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between h-16">
                <div class="flex items-center gap-3">
                    <div class="w-10 h-10 bg-brand-600 rounded-xl flex items-center justify-center text-white font-bold text-xl">
                        <i class="fas fa-share-nodes"></i>
                    </div>
                    <span class="text-xl font-bold text-gray-900">SocialSync</span>
                </div>
                <div class="flex items-center gap-4">
                    <div class="hidden md:flex items-center gap-2 px-3 py-1.5 bg-green-50 text-green-700 rounded-full text-sm font-medium">
                        <div class="w-2 h-2 bg-green-500 rounded-full status-dot"></div>
                        All Systems Operational
                    </div>
                    <button class="p-2 text-gray-500 hover:text-gray-700 relative">
                        <i class="fas fa-bell"></i>
                        <span class="absolute top-1 right-1 w-2 h-2 bg-red-500 rounded-full"></span>
                    </button>
                    <div class="w-8 h-8 bg-brand-100 rounded-full flex items-center justify-center text-brand-600 font-semibold">
                        JD
                    </div>
                </div>
            </div>
        </div>
    </nav>

    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
        <div class="grid grid-cols-1 lg:grid-cols-12 gap-8">
            
            <!-- Left Sidebar -->
            <div class="lg:col-span-3 space-y-6">
                <!-- Platform Status -->
                <div class="bg-white rounded-2xl shadow-sm border border-gray-200 p-6">
                    <h3 class="font-semibold text-gray-900 mb-4 flex items-center gap-2">
                        <i class="fas fa-plug text-brand-600"></i>
                        Connected Platforms
                    </h3>
                    <div class="space-y-3" id="platformList">
                        <!-- Instagram -->
                        <div class="flex items-center justify-between p-3 rounded-xl border border-gray-200 hover:border-pink-200 hover:bg-pink-50 transition-all cursor-pointer group" onclick="togglePlatform('instagram')">
                            <div class="flex items-center gap-3">
                                <div class="w-10 h-10 rounded-lg bg-gradient-to-br from-purple-600 via-pink-500 to-orange-400 flex items-center justify-center text-white">
                                    <i class="fab fa-instagram text-lg"></i>
                                </div>
                                <div>
                                    <div class="font-medium text-sm">Instagram</div>
                                    <div class="text-xs text-gray-500" id="instaStatus">Connected</div>
                                </div>
                            </div>
                            <div class="w-2 h-2 rounded-full bg-green-500" id="instaDot"></div>
                        </div>
                        
                        <!-- YouTube -->
                        <div class="flex items-center justify-between p-3 rounded-xl border border-gray-200 hover:border-red-200 hover:bg-red-50 transition-all cursor-pointer group" onclick="togglePlatform('youtube')">
                            <div class="flex items-center gap-3">
                                <div class="w-10 h-10 rounded-lg bg-red-600 flex items-center justify-center text-white">
                                    <i class="fab fa-youtube text-lg"></i>
                                </div>
                                <div>
                                    <div class="font-medium text-sm">YouTube</div>
                                    <div class="text-xs text-gray-500" id="ytStatus">Connected</div>
                                </div>
                            </div>
                            <div class="w-2 h-2 rounded-full bg-green-500" id="ytDot"></div>
                        </div>
                        
                        <!-- TikTok -->
                        <div class="flex items-center justify-between p-3 rounded-xl border border-gray-200 hover:border-gray-300 hover:bg-gray-50 transition-all cursor-pointer group" onclick="togglePlatform('tiktok')">
                            <div class="flex items-center gap-3">
                                <div class="w-10 h-10 rounded-lg bg-black flex items-center justify-center text-white">
                                    <i class="fab fa-tiktok text-lg"></i>
                                </div>
                                <div>
                                    <div class="font-medium text-sm">TikTok</div>
                                    <div class="text-xs text-gray-500" id="ttStatus">Connected</div>
                                </div>
                            </div>
                            <div class="w-2 h-2 rounded-full bg-green-500" id="ttDot"></div>
                        </div>
                        
                        <!-- Twitter -->
                        <div class="flex items-center justify-between p-3 rounded-xl border border-gray-200 hover:border-blue-200 hover:bg-blue-50 transition-all cursor-pointer group" onclick="togglePlatform('twitter')">
                            <div class="flex items-center gap-3">
                                <div class="w-10 h-10 rounded-lg bg-blue-400 flex items-center justify-center text-white">
                                    <i class="fab fa-twitter text-lg"></i>
                                </div>
                                <div>
                                    <div class="font-medium text-sm">Twitter</div>
                                    <div class="text-xs text-gray-500" id="twStatus">Not Connected</div>
                                </div>
                            </div>
                            <div class="w-2 h-2 rounded-full bg-gray-300" id="twDot"></div>
                        </div>
                    </div>
                    <button class="w-full mt-4 py-2 text-sm text-brand-600 font-medium hover:bg-brand-50 rounded-lg transition-colors" onclick="showAuthModal()">
                        <i class="fas fa-plus mr-1"></i> Add Platform
                    </button>
                </div>

                <!-- Rate Limits -->
                <div class="bg-white rounded-2xl shadow-sm border border-gray-200 p-6">
                    <h3 class="font-semibold text-gray-900 mb-4 flex items-center gap-2">
                        <i class="fas fa-gauge-high text-brand-600"></i>
                        API Rate Limits
                    </h3>
                    <div class="space-y-4">
                        <div>
                            <div class="flex justify-between text-sm mb-1">
                                <span class="text-gray-600">Instagram</span>
                                <span class="font-medium text-gray-900">45/60</span>
                            </div>
                            <div class="w-full bg-gray-200 rounded-full h-2">
                                <div class="bg-gradient-to-r from-purple-500 to-pink-500 h-2 rounded-full" style="width: 75%"></div>
                            </div>
                        </div>
                        <div>
                            <div class="flex justify-between text-sm mb-1">
                                <span class="text-gray-600">YouTube</span>
                                <span class="font-medium text-gray-900">28/100</span>
                            </div>
                            <div class="w-full bg-gray-200 rounded-full h-2">
                                <div class="bg-red-500 h-2 rounded-full" style="width: 28%"></div>
                            </div>
                        </div>
                        <div>
                            <div class="flex justify-between text-sm mb-1">
                                <span class="text-gray-600">TikTok</span>
                                <span class="font-medium text-gray-900">12/20</span>
                            </div>
                            <div class="w-full bg-gray-200 rounded-full h-2">
                                <div class="bg-black h-2 rounded-full" style="width: 60%"></div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Main Content -->
            <div class="lg:col-span-9 space-y-6">
                
                <!-- Upload Section -->
                <div class="bg-white rounded-2xl shadow-sm border border-gray-200 p-8">
                    <div class="text-center mb-6">
                        <h2 class="text-2xl font-bold text-gray-900 mb-2">Upload Your Videos</h2>
                        <p class="text-gray-600">Drag and drop your video files or click to browse. Supports MP4, MOV, AVI up to 2GB.</p>
                    </div>
                    
                    <div id="dropZone" class="border-3 border-dashed border-gray-300 rounded-2xl p-12 text-center transition-all duration-300 cursor-pointer hover:border-brand-400 hover:bg-brand-50" ondragover="handleDragOver(event)" ondragleave="handleDragLeave(event)" ondrop="handleDrop(event)" onclick="document.getElementById('fileInput').click()">
                        <input type="file" id="fileInput" multiple accept="video/*" class="hidden" onchange="handleFiles(this.files)">
                        <div class="w-20 h-20 bg-brand-100 rounded-full flex items-center justify-center mx-auto mb-4 text-brand-600 text-3xl">
                            <i class="fas fa-cloud-upload-alt"></i>
                        </div>
                        <p class="text-lg font-medium text-gray-900 mb-1">Drop your videos here</p>
                        <p class="text-sm text-gray-500">or click to browse files</p>
                    </div>

                    <!-- Upload Progress -->
                    <div id="uploadProgress" class="hidden mt-6 space-y-3">
                        <div class="flex items-center justify-between text-sm">
                            <span class="font-medium text-gray-700">Uploading...</span>
                            <span class="text-brand-600 font-semibold" id="progressPercent">0%</span>
                        </div>
                        <div class="w-full bg-gray-200 rounded-full h-3 overflow-hidden">
                            <div id="progressBar" class="bg-brand-600 h-3 rounded-full transition-all duration-300" style="width: 0%"></div>
                        </div>
                    </div>
                </div>

                <!-- Video Queue -->
                <div class="bg-white rounded-2xl shadow-sm border border-gray-200 p-6" id="videoQueue" style="display: none;">
                    <div class="flex items-center justify-between mb-6">
                        <h3 class="text-lg font-semibold text-gray-900">Video Queue (<span id="queueCount">0</span>)</h3>
                        <div class="flex gap-2">
                            <button onclick="clearQueue()" class="px-4 py-2 text-sm text-red-600 hover:bg-red-50 rounded-lg transition-colors">
                                <i class="fas fa-trash mr-1"></i> Clear All
                            </button>
                            <button onclick="scheduleAll()" class="px-4 py-2 text-sm bg-brand-600 text-white hover:bg-brand-700 rounded-lg transition-colors shadow-sm">
                                <i class="fas fa-calendar-check mr-1"></i> Schedule All
                            </button>
                        </div>
                    </div>
                    
                    <div id="videoList" class="space-y-4 max-h-96 overflow-y-auto scrollbar-hide">
                        <!-- Video items will be inserted here -->
                    </div>
                </div>

                <!-- Schedule Overview -->
                <div class="bg-white rounded-2xl shadow-sm border border-gray-200 p-6">
                    <div class="flex items-center justify-between mb-6">
                        <h3 class="text-lg font-semibold text-gray-900">Posting Schedule</h3>
                        <div class="flex gap-2">
                            <button onclick="viewMode='timeline'; renderSchedule()" class="px-3 py-1.5 text-sm rounded-lg bg-brand-100 text-brand-700 font-medium">
                                <i class="fas fa-stream mr-1"></i> Timeline
                            </button>
                            <button onclick="viewMode='calendar'; renderSchedule()" class="px-3 py-1.5 text-sm rounded-lg text-gray-600 hover:bg-gray-100">
                                <i class="fas fa-calendar mr-1"></i> Calendar
                            </button>
                        </div>
                    </div>
                    
                    <div id="scheduleContainer" class="space-y-4">
                        <!-- Schedule items will be inserted here -->
                    </div>
                </div>

                <!-- Error Log -->
                <div class="bg-white rounded-2xl shadow-sm border border-gray-200 p-6">
                    <div class="flex items-center justify-between mb-4">
                        <h3 class="text-lg font-semibold text-gray-900 flex items-center gap-2">
                            <i class="fas fa-shield-halved text-brand-600"></i>
                            Error Log & Activity
                        </h3>
                        <button onclick="clearLogs()" class="text-sm text-gray-500 hover:text-gray-700">
                            <i class="fas fa-eraser mr-1"></i> Clear
                        </button>
                    </div>
                    <div id="errorLog" class="space-y-2 max-h-48 overflow-y-auto scrollbar-hide">
                        <div class="flex items-start gap-3 p-3 rounded-lg bg-gray-50 text-sm">
                            <i class="fas fa-info-circle text-blue-500 mt-0.5"></i>
                            <div>
                                <div class="font-medium text-gray-900">System Ready</div>
                                <div class="text-gray-600">All platform connections verified successfully.</div>
                                <div class="text-xs text-gray-400 mt-1">Just now</div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Schedule Modal -->
    <div id="scheduleModal" class="fixed inset-0 bg-black/50 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white rounded-2xl shadow-2xl max-w-2xl w-full max-h-[90vh] overflow-y-auto">
            <div class="p-6 border-b border-gray-200">
                <div class="flex items-center justify-between">
                    <h3 class="text-xl font-bold text-gray-900">Schedule Distribution</h3>
                    <button onclick="closeScheduleModal()" class="text-gray-400 hover:text-gray-600">
                        <i class="fas fa-times text-xl"></i>
                    </button>
                </div>
            </div>
            <div class="p-6 space-y-6">
                <div id="scheduleVideoPreview" class="bg-gray-100 rounded-xl p-4 flex items-center gap-4">
                    <!-- Preview content -->
                </div>
                
                <div class="space-y-4">
                    <h4 class="font-semibold text-gray-900">Select Platforms & Times</h4>
                    
                    <div class="space-y-3" id="platformScheduleList">
                        <!-- Platform scheduling items -->
                    </div>
                </div>
                
                <div class="flex items-center gap-3 p-4 bg-yellow-50 rounded-xl border border-yellow-200">
                    <i class="fas fa-lightbulb text-yellow-600"></i>
                    <p class="text-sm text-yellow-800">Tip: Stagger your posts by at least 30 minutes to avoid rate limiting and maximize engagement.</p>
                </div>
            </div>
            <div class="p-6 border-t border-gray-200 flex justify-end gap-3">
                <button onclick="closeScheduleModal()" class="px-6 py-2.5 text-gray-700 hover:bg-gray-100 rounded-xl font-medium transition-colors">
                    Cancel
                </button>
                <button onclick="confirmSchedule()" class="px-6 py-2.5 bg-brand-600 text-white hover:bg-brand-700 rounded-xl font-medium transition-colors shadow-lg shadow-brand-500/30">
                    <i class="fas fa-check mr-2"></i>Confirm Schedule
                </button>
            </div>
        </div>
    </div>

    <!-- Auth Modal -->
    <div id="authModal" class="fixed inset-0 bg-black/50 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white rounded-2xl shadow-2xl max-w-md w-full p-6">
            <div class="text-center mb-6">
                <div class="w-16 h-16 bg-brand-100 rounded-full flex items-center justify-center mx-auto mb-4 text-brand-600 text-2xl">
                    <i class="fas fa-link"></i>
                </div>
                <h3 class="text-xl font-bold text-gray-900">Connect Platform</h3>
                <p class="text-gray-600 mt-2">Authenticate with your social media account to enable posting.</p>
            </div>
            
            <div class="space-y-3">
                <button onclick="simulateAuth('twitter')" class="w-full flex items-center gap-4 p-4 rounded-xl border-2 border-gray-200 hover:border-blue-400 hover:bg-blue-50 transition-all group">
                    <div class="w-12 h-12 rounded-lg bg-blue-400 flex items-center justify-center text-white text-xl">
                        <i class="fab fa-twitter"></i>
                    </div>
                    <div class="text-left">
                        <div class="font-semibold text-gray-900 group-hover:text-blue-600">Twitter / X</div>
                        <div class="text-sm text-gray-500">Connect account</div>
                    </div>
                    <i class="fas fa-chevron-right ml-auto text-gray-400"></i>
                </button>
                
                <button onclick="simulateAuth('facebook')" class="w-full flex items-center gap-4 p-4 rounded-xl border-2 border-gray-200 hover:border-blue-600 hover:bg-blue-50 transition-all group">
                    <div class="w-12 h-12 rounded-lg bg-blue-600 flex items-center justify-center text-white text-xl">
                        <i class="fab fa-facebook-f"></i>
                    </div>
                    <div class="text-left">
                        <div class="font-semibold text-gray-900 group-hover:text-blue-700">Facebook</div>
                        <div class="text-sm text-gray-500">Connect page</div>
                    </div>
                    <i class="fas fa-chevron-right ml-auto text-gray-400"></i>
                </button>
            </div>
            
            <button onclick="closeAuthModal()" class="w-full mt-4 py-3 text-gray-500 hover:text-gray-700 font-medium">
                Cancel
            </button>
        </div>
    </div>

    <script>
        // State Management
        let videos = [];
        let scheduledPosts = [];
        let viewMode = 'timeline';
        let currentEditingVideo = null;
        let platforms = {
            instagram: { connected: true, name: 'Instagram', icon: 'fab fa-instagram', color: 'from-purple-600 via-pink-500 to-orange-400' },
            youtube: { connected: true, name: 'YouTube', icon: 'fab fa-youtube', color: 'bg-red-600' },
            tiktok: { connected: true, name: 'TikTok', icon: 'fab fa-tiktok', color: 'bg-black' },
            twitter: { connected: false, name: 'Twitter', icon: 'fab fa-twitter', color: 'bg-blue-400' }
        };

        // Platform Toggle
        function togglePlatform(platform) {
            platforms[platform].connected = !platforms[platform].connected;
            updatePlatformUI();
            addLog(`${platforms[platform].name} ${platforms[platform].connected ? 'connected' : 'disconnected'}`, platforms[platform].connected ? 'success' : 'warning');
        }

        function updatePlatformUI() {
            Object.keys(platforms).forEach(key => {
                const p = platforms[key];
                const statusEl = document.getElementById(key === 'instagram' ? 'instaStatus' : key === 'youtube' ? 'ytStatus' : key === 'tiktok' ? 'ttStatus' : 'twStatus');
                const dotEl = document.getElementById(key === 'instagram' ? 'instaDot' : key === 'youtube' ? 'ytDot' : key === 'tiktok' ? 'ttDot' : 'twDot');
                
                if (statusEl) {
                    statusEl.textContent = p.connected ? 'Connected' : 'Not Connected';
                    statusEl.className = `text-xs ${p.connected ? 'text-green-600' : 'text-gray-500'}`;
                }
                if (dotEl) {
                    dotEl.className = `w-2 h-2 rounded-full ${p.connected ? 'bg-green-500' : 'bg-gray-300'}`;
                }
            });
        }

        // File Upload Handling
        function handleDragOver(e) {
            e.preventDefault();
            e.currentTarget.classList.add('drag-active');
        }

        function handleDragLeave(e) {
            e.currentTarget.classList.remove('drag-active');
        }

        function handleDrop(e) {
            e.preventDefault();
            e.currentTarget.classList.remove('drag-active');
            handleFiles(e.dataTransfer.files);
        }

        function handleFiles(files) {
            const validFiles = Array.from(files).filter(file => file.type.startsWith('video/'));
            
            if (validFiles.length === 0) {
                addLog('No valid video files selected', 'error');
                return;
            }

            // Simulate upload progress
            const progressContainer = document.getElementById('uploadProgress');
            const progressBar = document.getElementById('progressBar');
            const progressPercent = document.getElementById('progressPercent');
            
            progressContainer.classList.remove('hidden');
            let progress = 0;
            
            const interval = setInterval(() => {
                progress += Math.random() * 15;
                if (progress >= 100) {
                    progress = 100;
                    clearInterval(interval);
                    setTimeout(() => {
                        progressContainer.classList.add('hidden');
                        validFiles.forEach(file => addVideo(file));
                    }, 500);
                }
                progressBar.style.width = progress + '%';
                progressPercent.textContent = Math.round(progress) + '%';
            }, 200);
        }

        function addVideo(file) {
            const video = {
                id: Date.now() + Math.random(),
                file: file,
                name: file.name,
                size: formatFileSize(file.size),
                duration: '0:00',
                thumbnail: null,
                platforms: [],
                schedule: {}
            };
            
            // Generate thumbnail
            const url = URL.createObjectURL(file);
            const videoEl = document.createElement('video');
            videoEl.src = url;
            videoEl.onloadedmetadata = () => {
                video.duration = formatDuration(videoEl.duration);
                video.thumbnail = captureThumbnail(videoEl);
                updateVideoList();
            };
            
            videos.push(video);
            updateVideoList();
            addLog(`Uploaded "${file.name}"`, 'success');
        }

        function captureThumbnail(video) {
            const canvas = document.createElement('canvas');
            canvas.width = 320;
            canvas.height = 180;
            video.currentTime = 1;
            const ctx = canvas.getContext('2d');
            ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
            return canvas.toDataURL();
        }

        function formatFileSize(bytes) {
            if (bytes === 0) return '0 Bytes';
            const k = 1024;
            const sizes = ['Bytes', 'KB', 'MB', 'GB'];
            const i = Math.floor(Math.log(bytes) / Math.log(k));
            return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i];
        }

        function formatDuration(seconds) {
            const mins = Math.floor(seconds / 60);
            const secs = Math.floor(seconds % 60);
            return `${mins}:${secs.toString().padStart(2, '0')}`;
        }

        function updateVideoList() {
            const container = document.getElementById('videoList');
            const queueSection = document.getElementById('videoQueue');
            const countEl = document.getElementById('queueCount');
            
            countEl.textContent = videos.length;
            
            if (videos.length === 0) {
                queueSection.style.display = 'none';
                return;
            }
            
            queueSection.style.display = 'block';
            container.innerHTML = videos.map(video => `
                <div class="video-card bg-gray-50 rounded-xl p-4 border border-gray-200" id="video-${video.id}">
                    <div class="flex gap-4">
                        <div class="relative w-40 h-24 bg-gray-200 rounded-lg overflow-hidden flex-shrink-0">
                            ${video.thumbnail ? 
                                `<img src="${video.thumbnail}" class="w-full h-full object-cover" alt="Thumbnail">` : 
                                `<div class="w-full h-full flex items-center justify-center text-gray-400"><i class="fas fa-video text-2xl"></i></div>`
                            }
                            <div class="absolute bottom-1 right-1 bg-black/70 text-white text-xs px-1.5 py-0.5 rounded">
                                ${video.duration}
                            </div>
                        </div>
                        <div class="flex-1 min-w-0">
                            <div class="flex items-start justify-between mb-2">
                                <div>
                                    <h4 class="font-semibold text-gray-900 truncate" title="${video.name}">${video.name}</h4>
                                    <p class="text-sm text-gray-500">${video.size} • ${video.duration}</p>
                                </div>
                                <button onclick="removeVideo(${video.id})" class="text-gray-400 hover:text-red-500 transition-colors">
                                    <i class="fas fa-times"></i>
                                </button>
                            </div>
                            
                            <div class="flex items-center gap-2 mb-3">
                                ${Object.keys(platforms).filter(p => platforms[p].connected).map(p => `
                                    <div class="w-8 h-8 rounded-lg ${platforms[p].color.includes('gradient') ? 'bg-gradient-to-br ' + platforms[p].color : platforms[p].color} flex items-center justify-center text-white text-xs">
                                        <i class="${platforms[p].icon}"></i>
                                    </div>
                                `).join('') || '<span class="text-sm text-gray-400">No platforms connected</span>'}
                            </div>
                            
                            <div class="flex gap-2">
                                <button onclick="openScheduleModal(${video.id})" class="flex-1 py-2 bg-brand-600 text-white text-sm font-medium rounded-lg hover:bg-brand-700 transition-colors">
                                    <i class="fas fa-clock mr-1"></i> Schedule
                                </button>
                                <button onclick="postNow(${video.id})" class="px-4 py-2 bg-gray-200 text-gray-700 text-sm font-medium rounded-lg hover:bg-gray-300 transition-colors">
                                    <i class="fas fa-paper-plane"></i>
                                </button>
                            </div>
                        </div>
                    </div>
                    
                    ${video.platforms.length > 0 ? `
                        <div class="mt-3 pt-3 border-t border-gray-200">
                            <div class="flex flex-wrap gap-2">
                                ${video.platforms.map(p => `
                                    <div class="flex items-center gap-1.5 px-2.5 py-1 rounded-full bg-gray-200 text-xs font-medium text-gray-700">
                                        <i class="${platforms[p].icon}"></i>
                                        ${platforms[p].name}
                                        <span class="text-gray-500">${video.schedule[p] || 'Now'}</span>
                                    </div>
                                `).join('')}
                            </div>
                        </div>
                    ` : ''}
                </div>
            `).join('');
        }

        function removeVideo(id) {
            videos = videos.filter(v => v.id !== id);
            updateVideoList();
            addLog('Video removed from queue', 'info');
        }

        function clearQueue() {
            videos = [];
            updateVideoList();
            addLog('Queue cleared', 'info');
        }

        // Scheduling
        function openScheduleModal(videoId) {
            currentEditingVideo = videos.find(v => v.id === videoId);
            if (!currentEditingVideo) return;
            
            const modal = document.getElementById('scheduleModal');
            const preview = document.getElementById('scheduleVideoPreview');
            const platformList = document.getElementById('platformScheduleList');
            
            preview.innerHTML = `
                <div class="w-24 h-16 bg-gray-200 rounded-lg overflow-hidden flex-shrink-0">
                    ${currentEditingVideo.thumbnail ? 
                        `<img src="${currentEditingVideo.thumbnail}" class="w-full h-full object-cover">` : 
                        `<div class="w-full h-full flex items-center justify-center text-gray-400"><i class="fas fa-video"></i></div>`
                    }
                </div>
                <div>
                    <div class="font-semibold text-gray-900">${currentEditingVideo.name}</div>
                    <div class="text-sm text-gray-500">${currentEditingVideo.size} • ${currentEditingVideo.duration}</div>
                </div>
            `;
            
            const connectedPlatforms = Object.keys(platforms).filter(p => platforms[p].connected);
            
            platformList.innerHTML = connectedPlatforms.map(p => {
                const platform = platforms[p];
                const isSelected = currentEditingVideo.platforms.includes(p);
                const scheduledTime = currentEditingVideo.schedule[p] || '';
                
                return `
                    <div class="flex items-center gap-4 p-4 rounded-xl border-2 ${isSelected ? 'border-brand-500 bg-brand-50' : 'border-gray-200'} transition-all cursor-pointer" onclick="togglePlatformSchedule('${p}')">
                        <input type="checkbox" ${isSelected ? 'checked' : ''} class="w-5 h-5 text-brand-600 rounded focus:ring-brand-500" onchange="togglePlatformSchedule('${p}')">
                        <div class="w-10 h-10 rounded-lg ${platform.color.includes('gradient') ? 'bg-gradient-to-br ' + platform.color : platform.color} flex items-center justify-center text-white">
                            <i class="${platform.icon}"></i>
                        </div>
                        <div class="flex-1">
                            <div class="font-semibold text-gray-900">${platform.name}</div>
                            <div class="text-sm text-gray-500">Schedule post time</div>
                        </div>
                        <input type="datetime-local" 
                            value="${scheduledTime}" 
                            onchange="updateScheduleTime('${p}', this.value)"
                            onclick="event.stopPropagation()"
                            class="px-3 py-2 border border-gray-300 rounded-lg text-sm focus:ring-2 focus:ring-brand-500 focus:border-brand-500"
                            ${!isSelected ? 'disabled' : ''}
                        >
                    </div>
                `;
            }).join('');
            
            if (connectedPlatforms.length === 0) {
                platformList.innerHTML = `
                    <div class="text-center py-8 text-gray-500">
                        <i class="fas fa-plug text-4xl mb-3 text-gray-300"></i>
                        <p>No platforms connected. Please connect a platform first.</p>
                    </div>
                `;
            }
            
            modal.classList.remove('hidden');
        }

        function togglePlatformSchedule(platform) {
            const idx = currentEditingVideo.platforms.indexOf(platform);
            if (idx > -1) {
                currentEditingVideo.platforms.splice(idx, 1);
                delete currentEditingVideo.schedule[platform];
            } else {
                currentEditingVideo.platforms.push(platform);
                // Default to now + 1 hour
                const now = new Date();
                now.setHours(now.getHours() + 1);
                currentEditingVideo.schedule[platform] = now.toISOString().slice(0, 16);
            }
            openScheduleModal(currentEditingVideo.id);
        }

        function updateScheduleTime(platform, time) {
            currentEditingVideo.schedule[platform] = time;
        }

        function closeScheduleModal() {
            document.getElementById('scheduleModal').classList.add('hidden');
            currentEditingVideo = null;
        }

        function confirmSchedule() {
            if (currentEditingVideo.platforms.length === 0) {
                addLog('Please select at least one platform', 'error');
                return;
            }
            
            // Add to scheduled posts
            currentEditingVideo.platforms.forEach(platform => {
                scheduledPosts.push({
                    id: Date.now() + Math.random(),
                    videoId: currentEditingVideo.id,
                    videoName: currentEditingVideo.name,
                    platform: platform,
                    scheduledTime: currentEditingVideo.schedule[platform],
                    status: 'pending'
                });
            });
            
            addLog(`Scheduled "${currentEditingVideo.name}" for ${currentEditingVideo.platforms.length} platforms`, 'success');
            closeScheduleModal();
            updateVideoList();
            renderSchedule();
        }

        function scheduleAll() {
            videos.forEach(video => {
                if (video.platforms.length === 0) {
                    // Auto-schedule for all connected platforms
                    const connected = Object.keys(platforms).filter(p => platforms[p].connected);
                    video.platforms = connected;
                    const now = new Date();
                    connected.forEach((p, i) => {
                        const time = new Date(now);
                        time.setMinutes(time.getMinutes() + (i * 30));
                        video.schedule[p] = time.toISOString().slice(0, 16);
                        
                        scheduledPosts.push({
                            id: Date.now() + Math.random(),
                            videoId: video.id,
                            videoName: video.name,
                            platform: p,
                            scheduledTime: video.schedule[p],
                            status: 'pending'
                        });
                    });
                }
            });
            
            updateVideoList();
            renderSchedule();
            addLog(`Scheduled ${videos.length} videos across all platforms`, 'success');
        }

        function postNow(videoId) {
            const video = videos.find(v => v.id === videoId);
            if (!video) return;
            
            const connected = Object.keys(platforms).filter(p => platforms[p].connected);
            if (connected.length === 0) {
                addLog('No platforms connected', 'error');
                return;
            }
            
            // Simulate posting
            addLog(`Publishing "${video.name}"...`, 'info');
            
            connected.forEach((platform, index) => {
                setTimeout(() => {
                    if (Math.random() > 0.1) { // 90% success rate
                        addLog(`Successfully posted to ${platforms[platform].name}`, 'success');
                    } else {
                        addLog(`Rate limit hit on ${platforms[platform].name}. Retrying...`, 'warning');
                        setTimeout(() => {
                            addLog(`Retry successful for ${platforms[platform].name}`, 'success');
                        }, 2000);
                    }
                }, index * 1500);
            });
        }

        // Schedule Rendering
        function renderSchedule() {
            const container = document.getElementById('scheduleContainer');
            
            if (scheduledPosts.length === 0) {
                container.innerHTML = `
                    <div class="text-center py-12 text-gray-400">
                        <i class="fas fa-calendar-plus text-4xl mb-3"></i>
                        <p>No scheduled posts yet</p>
                    </div>
                `;
                return;
            }
            
            // Sort by time
            const sorted = [...scheduledPosts].sort((a, b) => new Date(a.scheduledTime) - new Date(b.scheduledTime));
            
            if (viewMode === 'timeline') {
                container.innerHTML = `
                    <div class="space-y-0">
                        ${sorted.map((post, index) => {
                            const platform = platforms[post.platform];
                            const time = new Date(post.scheduledTime);
                            const isPast = time < new Date();
                            
                            return `
                                <div class="timeline-item pb-6">
                                    <div class="flex items-start justify-between bg-gray-50 rounded-xl p-4 border border-gray-200 ${isPast ? 'opacity-60' : ''}">
                                        <div class="flex items-start gap-3">
                                            <div class="w-10 h-10 rounded-lg ${platform.color.includes('gradient') ? 'bg-gradient-to-br ' + platform.color : platform.color} flex items-center justify-center text-white flex-shrink-0">
                                                <i class="${platform.icon}"></i>
                                            </div>
                                            <div>
                                                <div class="font-semibold text-gray-900">${post.videoName}</div>
                                                <div class="text-sm text-gray-600">${platform.name}</div>
                                                <div class="text-xs text-gray-400 mt-1">
                                                    <i class="far fa-clock mr-1"></i>
                                                    ${time.toLocaleString()}
                                                </div>
                                            </div>
                                        </div>
                                        <div class="flex items-center gap-2">
                                            <span class="px-2.5 py-1 rounded-full text-xs font-medium ${isPast ? 'bg-green-100 text-green-700' : 'bg-blue-100 text-blue-700'}">
                                                ${isPast ? 'Posted' : 'Pending'}
                                            </span>
                                            <button onclick="removeScheduledPost(${post.id})" class="text-gray-400 hover:text-red-500 transition-colors">
                                                <i class="fas fa-times"></i>
                                            </button>
                                        </div>
                                    </div>
                                </div>
                            `;
                        }).join('')}
                    </div>
                `;
            } else {
                // Calendar view (simplified)
                const today = new Date();
                const days = [];
                for (let i = 0; i < 7; i++) {
                    const d = new Date(today);
                    d.setDate(d.getDate() + i);
                    days.push(d);
                }
                
                container.innerHTML = `
                    <div class="grid grid-cols-7 gap-2">
                        ${days.map(day => {
                            const dayPosts = sorted.filter(p => {
                                const pDate = new Date(p.scheduledTime);
                                return pDate.toDateString() === day.toDateString();
                            });
                            
                            return `
                                <div class="border border-gray-200 rounded-xl p-3 min-h-[120px] ${day.toDateString() === today.toDateString() ? 'bg-brand-50 border-brand-200' : 'bg-white'}">
                                    <div class="text-xs font-semibold text-gray-500 mb-2">
                                        ${day.toLocaleDateString('en-US', { weekday: 'short', month: 'short', day: 'numeric' })}
                                    </div>
                                    <div class="space-y-1">
                                        ${dayPosts.map(post => {
                                            const platform = platforms[post.platform];
                                            return `
                                                <div class="text-xs p-1.5 rounded bg-gray-100 truncate flex items-center gap-1">
                                                    <i class="${platform.icon} text-[10px]"></i>
                                                    ${post.videoName.substring(0, 15)}...
                                                </div>
                                            `;
                                        }).join('')}
                                    </div>
                                </div>
                            `;
                        }).join('')}
                    </div>
                `;
            }
        }

        function removeScheduledPost(id) {
            scheduledPosts = scheduledPosts.filter(p => p.id !== id);
            renderSchedule();
            addLog('Scheduled post removed', 'info');
        }

        // Error Logging
        function addLog(message, type = 'info') {
            const container = document.getElementById('errorLog');
            const icons = {
                info: 'fa-info-circle text-blue-500',
                success: 'fa-check-circle text-green-500',
                warning: 'fa-exclamation-triangle text-yellow-500',
                error: 'fa-times-circle text-red-500'
            };
            
            const entry = document.createElement('div');
            entry.className = 'flex items-start gap-3 p-3 rounded-lg bg-gray-50 text-sm animate-fade-in';
            entry.innerHTML = `
                <i class="fas ${icons[type]} mt-0.5"></i>
                <div class="flex-1">
                    <div class="font-medium text-gray-900">${type.charAt(0).toUpperCase() + type.slice(1)}</div>
                    <div class="text-gray-600">${message}</div>
                    <div class="text-xs text-gray-400 mt-1">${new Date().toLocaleTimeString()}</div>
                </div>
            `;
            
            container.insertBefore(entry, container.firstChild);
            
            // Keep only last 20 logs
            while (container.children.length > 20) {
                container.removeChild(container.lastChild);
            }
        }

        function clearLogs() {
            document.getElementById('errorLog').innerHTML = '';
            addLog('Logs cleared', 'info');
        }

        // Auth Modal
        function showAuthModal() {
            document.getElementById('authModal').classList.remove('hidden');
        }

        function closeAuthModal() {
            document.getElementById('authModal').classList.add('hidden');
        }

        function simulateAuth(platform) {
            addLog(`Authenticating with ${platforms[platform]?.name || platform}...`, 'info');
            
            setTimeout(() => {
                platforms[platform].connected = true;
                updatePlatformUI();
                closeAuthModal();
                addLog(`Successfully connected to ${platforms[platform]?.name || platform}`, 'success');
            }, 1500);
        }

        // Auto-simulate some activity
        setInterval(() => {
            if (Math.random() > 0.7 && scheduledPosts.length > 0) {
                const pending = scheduledPosts.filter(p => p.status === 'pending' && new Date(p.scheduledTime) < new Date());
                if (pending.length > 0) {
                    const post = pending[0];
                    post.status = 'posted';
                    addLog(`Auto-published "${post.videoName}" to ${platforms[post.platform].name}`, 'success');
                    renderSchedule();
                }
            }
        }, 10000);

        // Initialize
        updatePlatformUI();
        renderSchedule();
    </script>
</body>
</html>

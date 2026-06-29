# WeBuild-tracking-Dashboard
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>WeBuild OS - Enterprise Suite</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Inter', sans-serif; background-color: #f8fafc; }
        .fade-in { animation: fadeIn 0.3s ease-out forwards; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        
        /* Custom Scrollbar */
        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 10px; }
        ::-webkit-scrollbar-thumb:hover { background: #94a3b8; }

        .glass-card { background: white; border: 1px solid #e2e8f0; box-shadow: 0 1px 3px rgba(0,0,0,0.05); }
        
        /* Google Button Style */
        .btn-google { background: #ffffff; border: 1px solid #dadce0; color: #3c4043; }
        .btn-google:hover { background: #f7f8f8; border-color: #d2d3d7; }
    </style>
</head>
<body class="text-slate-800 antialiased h-screen overflow-hidden flex flex-col">

    <!-- AUTH SCREEN -->
    <div id="auth-screen" class="fixed inset-0 z-50 bg-slate-900 flex items-center justify-center p-4">
        <div class="w-full max-w-md bg-white rounded-2xl shadow-2xl overflow-hidden fade-in">
            <div class="p-8 text-center border-b border-slate-100">
                <div class="w-16 h-16 bg-blue-600 rounded-xl mx-auto mb-4 flex items-center justify-center text-white font-bold text-2xl shadow-lg shadow-blue-500/30">W</div>
                <h1 class="text-2xl font-bold text-slate-900">WeBuild OS</h1>
                <p class="text-slate-500 mt-2 text-sm">Enterprise Management & E-Commerce</p>
            </div>
            
            <div class="p-8 space-y-4">
                <!-- Simulated Role Selection for Demo -->
                <div>
                    <label class="block text-xs font-semibold text-slate-500 uppercase tracking-wider mb-2">Simulate Login As</label>
                    <select id="role-select" class="w-full p-3 bg-slate-50 border border-slate-200 rounded-lg focus:ring-2 focus:ring-blue-500 outline-none transition-all">
                        <option value="owner">Owner / Admin</option>
                        <option value="employee">Employee / Warehouse</option>
                        <option value="driver">Delivery Driver</option>
                        <option value="customer">Customer (Store Shopper)</option>
                    </select>
                </div>
                
                <div class="relative my-6">
                    <div class="absolute inset-0 flex items-center"><div class="w-full border-t border-slate-200"></div></div>
                    <div class="relative flex justify-center text-sm"><span class="px-2 bg-white text-slate-500">Or continue with</span></div>
                </div>

                <!-- Google Sign In Simulation -->
                <button onclick="handleLogin('google')" class="w-full btn-google font-medium py-3 rounded-lg transition-all flex items-center justify-center gap-3 shadow-sm">
                    <svg class="w-5 h-5" viewBox="0 0 24 24"><path fill="#4285F4" d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z"/><path fill="#34A853" d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z"/><path fill="#FBBC05" d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l2.85-2.22.81-.62z"/><path fill="#EA4335" d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z"/></svg>
                    Sign in with Google
                </button>

                <button onclick="handleLogin('email')" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-3 rounded-lg transition-all shadow-lg shadow-blue-600/20 flex items-center justify-center gap-2">
                    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z"/></svg>
                    Sign in with Email
                </button>
                
                <p class="text-center text-xs text-slate-400 mt-4">Protected by WeBuild Security Protocol v3.0</p>
            </div>
        </div>
    </div>

    <!-- MAIN APP LAYOUT -->
    <div id="app-layout" class="hidden flex-1 flex h-screen overflow-hidden">
        
        <!-- SIDEBAR NAVIGATION -->
        <aside id="sidebar" class="w-72 bg-slate-900 text-white flex-shrink-0 flex flex-col h-full transition-all duration-300 z-20 hidden md:flex">
            <div class="p-6 flex items-center gap-3 border-b border-slate-800">
                <div class="w-8 h-8 bg-blue-500 rounded-lg flex items-center justify-center font-bold text-sm">W</div>
                <span class="font-semibold text-lg tracking-tight">WeBuild OS</span>
            </div>
            
            <nav id="nav-menu" class="flex-1 overflow-y-auto p-4 space-y-1">
                <!-- Nav items injected via JS based on role -->
            </nav>

            <div class="p-4 border-t border-slate-800">
                <div class="flex items-center gap-3 p-3 rounded-xl bg-slate-800/50 border border-slate-700 relative">
                    <img id="user-avatar" src="" class="w-10 h-10 rounded-full border-2 border-slate-600" alt="User">
                    <div class="flex-1 min-w-0">
                        <p id="user-name" class="text-sm font-medium text-white truncate"></p>
                        <p id="user-role-display" class="text-[10px] text-slate-400 uppercase tracking-wider truncate"></p>
                    </div>
                    <button onclick="logout()" class="text-slate-400 hover:text-white p-2 transition-colors" title="Logout">
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 16l4-4m0 0l-4-4m-8 14h8"/></svg>
                    </button>
                </div>
            </div>
        </aside>

        <!-- MAIN CONTENT AREA -->
        <main class="flex-1 flex flex-col h-full overflow-hidden relative">
            <!-- Mobile Header -->
            <header class="md:hidden bg-white border-b border-slate-200 p-4 flex justify-between items-center z-10">
                <div class="flex items-center gap-2">
                    <div class="w-8 h-8 bg-blue-600 rounded-lg flex items-center justify-center text-white font-bold text-xs">W</div>
                    <span class="font-bold text-slate-800">WeBuild OS</span>
                </div>
                <button onclick="toggleMobileMenu()" class="text-slate-600"><svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"/></svg></button>
            </header>

            <!-- Scrollable Content -->
            <div id="content-area" class="flex-1 overflow-y-auto p-4 md:p-8 bg-slate-50/50 relative">
                <!-- Dynamic Content Injected Here -->
            </div>
        </main>
    </div>

    <!-- JAVASCRIPT LOGIC -->
    <script>
        // --- MOCK DATA & STATE ---
        const ROLES = { OWNER: 'owner', EMPLOYEE: 'employee', DRIVER: 'driver', CUSTOMER: 'customer' };
        
        const USERS_DB = [
            { id: 'u1', name: 'Sarah Jenkins', email: 'sarah@webuild.os', role: ROLES.OWNER, avatar: 'https://ui-avatars.com/api/?name=Sarah+J&background=0f172a&color=fff', status: 'active' },
            { id: 'u2', name: 'Jessica Torres', email: 'jessica@webuild.os', role: ROLES.EMPLOYEE, avatar: 'https://ui-avatars.com/api/?name=Jessica+T&background=10b981&color=fff', status: 'active' },
            { id: 'u3', name: 'David Okonjo', email: 'david@webuild.os', role: ROLES.DRIVER, avatar: 'https://ui-avatars.com/api/?name=David+O&background=f59e0b&color=fff', status: 'active' },
            { id: 'u4', name: 'Michael Chen', email: 'michael@example.com', role: ROLES.CUSTOMER, avatar: 'https://ui-avatars.com/api/?name=Michael+C&background=3b82f6&color=fff', status: 'active' },
            { id: 'u5', name: 'Former Worker', email: 'former@webuild.os', role: ROLES.EMPLOYEE, avatar: 'https://ui-avatars.com/api/?name=F+W&background=ef4444&color=fff', status: 'banned' }
        ];

        const STORE_PRODUCTS = [
            { id: 'p1', name: 'Premium Web Hosting', price: 29.99, image: 'https://images.unsplash.com/photo-1558494949-ef010cbdcc31?auto=format&fit=crop&w=400&q=80', category: 'Services' },
            { id: 'p2', name: 'E-Commerce Starter Kit', price: 149.00, image: 'https://images.unsplash.com/photo-1563013544-824ae1b704d3?auto=format&fit=crop&w=400&q=80', category: 'Packages' },
            { id: 'p3', name: 'SEO Optimization Audit', price: 75.00, image: 'https://images.unsplash.com/photo-1460925895917-afdab827c52f?auto=format&fit=crop&w=400&q=80', category: 'Services' },
            { id: 'p4', name: 'Custom Domain (.com)', price: 12.99, image: 'https://images.unsplash.com/photo-1516321318423-f06f85e504b3?auto=format&fit=crop&w=400&q=80', category: 'Domains' },
            { id: 'p5', name: 'Monthly Maintenance', price: 49.99, image: 'https://images.unsplash.com/photo-1581091226825-a6a2a5aee158?auto=format&fit=crop&w=400&q=80', category: 'Services' },
            { id: 'p6', name: 'Logo Design Package', price: 199.00, image: 'https://images.unsplash.com/photo-1626785774573-4b799315345d?auto=format&fit=crop&w=400&q=80', category: 'Design' }
        ];

        let currentUser = null;
        let cart = [];

        // --- AUTHENTICATION HANDLERS ---
        function handleLogin(method) {
            const select = document.getElementById('role-select');
            const role = select.value;
            
            // Find or create user in DB
            let user = USERS_DB.find(u => u.role === role && u.status === 'active');
            if(!user) {
                alert("This account has been banned/removed by admin.");
                return;
            }

            currentUser = user;
            
            setTimeout(() => {
                document.getElementById('auth-screen').classList.add('hidden');
                document.getElementById('app-layout').classList.remove('hidden');
                initializeDashboard();
            }, 600);
        }

        function logout() {
            currentUser = null;
            cart = [];
            document.getElementById('app-layout').classList.add('hidden');
            document.getElementById('auth-screen').classList.remove('hidden');
        }

        // --- DASHBOARD INITIALIZATION ---
        function initializeDashboard() {
            document.getElementById('user-name').innerText = currentUser.name;
            document.getElementById('user-role-display').innerText = currentUser.role.toUpperCase();
            document.getElementById('user-avatar').src = currentUser.avatar;

            renderNavigation();
            renderContent(getDefaultView());
        }

        function getDefaultView() {
            if(currentUser.role === ROLES.CUSTOMER) return 'E-Commerce Store';
            if(currentUser.role === ROLES.OWNER) return 'Admin Panel';
            if(currentUser.role === ROLES.DRIVER) return 'Active Delivery';
            return 'My Schedule';
        }

        function renderNavigation() {
            const navContainer = document.getElementById('nav-menu');
            navContainer.innerHTML = '';
            
            let items = [];
            if(currentUser.role === ROLES.OWNER) {
                items = [
                    { label: 'Admin Panel', icon: 'shield-check', active: true },
                    { label: 'User Management', icon: 'users' },
                    { label: 'Store Products', icon: 'shopping-bag' },
                    { label: 'Schedules', icon: 'calendar' },
                    { label: 'Messaging', icon: 'chat-alt-2' }
                ];
            } else if (currentUser.role === ROLES.CUSTOMER) {
                items = [
                    { label: 'E-Commerce Store', icon: 'shopping-bag', active: true },
                    { label: 'My Orders', icon: 'document-text' },
                    { label: 'Support Chat', icon: 'chat-alt-2' }
                ];
            } else if (currentUser.role === ROLES.EMPLOYEE) {
                items = [
                    { label: 'My Schedule', icon: 'calendar', active: true },
                    { label: 'Tasks', icon: 'check-circle' },
                    { label: 'Messages', icon: 'chat-alt-2' }
                ];
            } else if (currentUser.role === ROLES.DRIVER) {
                items = [
                    { label: 'Active Delivery', icon: 'location-marker', active: true },
                    { label: 'My Schedule', icon: 'calendar' },
                    { label: 'Messages', icon: 'chat-alt-2' }
                ];
            }

            items.forEach((item) => {
                const isActive = item.active ? 'bg-blue-600 text-white shadow-lg shadow-blue-900/20' : 'text-slate-400 hover:bg-slate-800 hover:text-white';
                const link = document.createElement('a');
                link.href = '#';
                link.className = `flex items-center gap-3 px-3 py-2.5 rounded-lg transition-all text-sm font-medium ${isActive}`;
                link.innerHTML = `${getIcon(item.icon)} ${item.label}`;
                link.onclick = (e) => {
                    e.preventDefault();
                    Array.from(navContainer.children).forEach(c => c.className = c.className.replace('bg-blue-600 text-white shadow-lg shadow-blue-900/20', 'text-slate-400 hover:bg-slate-800 hover:text-white'));
                    link.className = `flex items-center gap-3 px-3 py-2.5 rounded-lg transition-all text-sm font-medium bg-blue-600 text-white shadow-lg shadow-blue-900/20`;
                    renderContent(item.label);
                };
                navContainer.appendChild(link);
            });
        }

        // --- CONTENT RENDERING ENGINE ---
        function renderContent(viewTitle) {
            const container = document.getElementById('content-area');
            container.innerHTML = '';
            
            const header = document.createElement('div');
            header.className = 'mb-8 fade-in flex justify-between items-end';
            
            // Special header for Store to show Cart
            if(viewTitle === 'E-Commerce Store') {
                header.innerHTML = `
                    <div><h1 class="text-2xl font-bold text-slate-900">${viewTitle}</h1><p class="text-slate-500 text-sm mt-1">Browse our digital services and packages.</p></div>
                    <button onclick="toggleCart()" class="relative bg-white p-3 rounded-xl border border-slate-200 hover:border-blue-300 transition-colors shadow-sm">
                        ${getIcon('shopping-cart')}
                        ${cart.length > 0 ? `<span class="absolute -top-2 -right-2 w-5 h-5 bg-red-500 text-white text-[10px] font-bold rounded-full flex items-center justify-center">${cart.length}</span>` : ''}
                    </button>
                `;
            } else {
                header.innerHTML = `<div><h1 class="text-2xl font-bold text-slate-900">${viewTitle}</h1><p class="text-slate-500 text-sm mt-1">Welcome back, ${currentUser.name}.</p></div>`;
            }
            container.appendChild(header);

            switch(currentUser.role) {
                case ROLES.OWNER: renderOwnerView(viewTitle, container); break;
                case ROLES.EMPLOYEE: renderEmployeeView(viewTitle, container); break;
                case ROLES.DRIVER: renderDriverView(viewTitle, container); break;
                case ROLES.CUSTOMER: renderCustomerView(viewTitle, container); break;
            }
        }

        // --- VIEW RENDERERS ---

        function renderOwnerView(title, container) {
            if(title === 'Admin Panel') {
                container.innerHTML += `
                    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8 fade-in">
                        ${statCard('Total Users', USERS_DB.length, 'Across all roles', 'blue', 'users')}
                        ${statCard('Active Orders', '42', '12 Delivered today', 'green', 'shopping-bag')}
                        ${statCard('Store Revenue', '$12,450', '+18% this month', 'emerald', 'currency-dollar')}
                        ${statCard('Banned Users', USERS_DB.filter(u=>u.status==='banned').length, 'Needs review', 'red', 'ban')}
                    </div>
                    
                    <div class="grid lg:grid-cols-2 gap-6 fade-in">
                        <div class="glass-card rounded-xl p-6">
                            <h3 class="font-bold text-slate-800 mb-4">System Health</h3>
                            <div class="space-y-4">
                                ${progressBar('Server Load', 45, 'green')}
                                ${progressBar('Database Storage', 72, 'amber')}
                                ${progressBar('API Latency', 12, 'green')}
                            </div>
                        </div>
                        <div class="glass-card rounded-xl p-6">
                            <h3 class="font-bold text-slate-800 mb-4">Quick Actions</h3>
                            <div class="grid grid-cols-2 gap-3">
                                <button onclick="renderContent('User Management')" class="p-4 bg-blue-50 hover:bg-blue-100 rounded-xl text-left transition-colors border border-blue-100">
                                    <div class="text-blue-600 mb-2">${getIcon('users', 'w-6 h-6')}</div>
                                    <p class="font-bold text-slate-800 text-sm">Manage Users</p>
                                </button>
                                <button onclick="renderContent('Store Products')" class="p-4 bg-purple-50 hover:bg-purple-100 rounded-xl text-left transition-colors border border-purple-100">
                                    <div class="text-purple-600 mb-2">${getIcon('shopping-bag', 'w-6 h-6')}</div>
                                    <p class="font-bold text-slate-800 text-sm">Edit Store</p>
                                </button>
                            </div>
                        </div>
                    </div>
                `;
            } else if (title === 'User Management') {
                container.innerHTML += `
                    <div class="glass-card rounded-xl overflow-hidden fade-in">
                        <table class="w-full text-left text-sm">
                            <thead class="bg-slate-50 border-b border-slate-200">
                                <tr>
                                    <th class="p-4 font-semibold text-slate-600">User</th>
                                    <th class="p-4 font-semibold text-slate-600">Role</th>
                                    <th class="p-4 font-semibold text-slate-600">Status</th>
                                    <th class="p-4 font-semibold text-slate-600 text-right">Actions</th>
                                </tr>
                            </thead>
                            <tbody class="divide-y divide-slate-100">
                                ${USERS_DB.map(u => `
                                    <tr class="hover:bg-slate-50 transition-colors">
                                        <td class="p-4 flex items-center gap-3">
                                            <img src="${u.avatar}" class="w-8 h-8 rounded-full">
                                            <div>
                                                <p class="font-medium text-slate-900">${u.name}</p>
                                                <p class="text-xs text-slate-500">${u.email}</p>
                                            </div>
                                        </td>
                                        <td class="p-4"><span class="px-2 py-1 bg-slate-100 text-slate-600 rounded text-xs font-bold uppercase">${u.role}</span></td>
                                        <td class="p-4">
                                            <span class="px-2 py-1 rounded-full text-xs font-bold ${u.status === 'active' ? 'bg-emerald-100 text-emerald-700' : 'bg-red-100 text-red-700'}">
                                                ${u.status.toUpperCase()}
                                            </span>
                                        </td>
                                        <td class="p-4 text-right">
                                            ${u.status === 'active' ? 
                                                `<button onclick="banUser('${u.id}')" class="text-red-600 hover:bg-red-50 px-3 py-1.5 rounded-lg text-xs font-bold transition-colors border border-transparent hover:border-red-200">Ban / Remove</button>` : 
                                                `<button onclick="unbanUser('${u.id}')" class="text-emerald-600 hover:bg-emerald-50 px-3 py-1.5 rounded-lg text-xs font-bold transition-colors border border-transparent hover:border-emerald-200">Restore</button>`
                                            }
                                        </td>
                                    </tr>
                                `).join('')}
                            </tbody>
                        </table>
                    </div>
                `;
            } else if (title === 'Store Products') {
                container.innerHTML += `
                    <div class="grid md:grid-cols-2 lg:grid-cols-3 gap-6 fade-in">
                        ${STORE_PRODUCTS.map(p => `
                            <div class="glass-card rounded-xl overflow-hidden group">
                                <div class="h-40 bg-slate-200 relative overflow-hidden">
                                    <img src="${p.image}" class="w-full h-full object-cover group-hover:scale-105 transition-transform duration-500">
                                    <div class="absolute top-3 right-3 bg-white/90 backdrop-blur px-2 py-1 rounded-lg text-xs font-bold text-slate-800">$${p.price}</div>
                                </div>
                                <div class="p-5">
                                    <p class="text-[10px] font-bold text-blue-600 uppercase tracking-widest mb-1">${p.category}</p>
                                    <h3 class="font-bold text-slate-900 mb-3">${p.name}</h3>
                                    <div class="flex gap-2">
                                        <button class="flex-1 bg-slate-900 text-white py-2 rounded-lg text-xs font-bold hover:bg-slate-800 transition-colors">Edit</button>
                                        <button class="px-3 bg-red-50 text-red-600 rounded-lg hover:bg-red-100 transition-colors">${getIcon('trash', 'w-4 h-4')}</button>
                                    </div>
                                </div>
                            </div>
                        `).join('')}
                        <button class="glass-card rounded-xl border-dashed border-2 border-slate-300 flex flex-col items-center justify-center p-8 text-slate-400 hover:border-blue-400 hover:text-blue-500 transition-all cursor-pointer h-full min-h-[250px]">
                            ${getIcon('plus', 'w-8 h-8 mb-2')}
                            <span class="font-bold text-sm">Add New Product</span>
                        </button>
                    </div>
                `;
            }
        }

        function renderCustomerView(title, container) {
            if(title === 'E-Commerce Store') {
                container.innerHTML += `
                    <div class="grid md:grid-cols-2 lg:grid-cols-3 gap-6 fade-in">
                        ${STORE_PRODUCTS.map(p => `
                            <div class="glass-card rounded-xl overflow-hidden group hover:shadow-lg transition-all duration-300">
                                <div class="h-48 bg-slate-200 relative overflow-hidden">
                                    <img src="${p.image}" class="w-full h-full object-cover group-hover:scale-105 transition-transform duration-500">
                                </div>
                                <div class="p-5">
                                    <p class="text-[10px] font-bold text-blue-600 uppercase tracking-widest mb-1">${p.category}</p>
                                    <h3 class="font-bold text-slate-900 mb-2">${p.name}</h3>
                                    <p class="text-xl font-bold text-slate-800 mb-4">$${p.price.toFixed(2)}</p>
                                    <button onclick="addToCart('${p.id}')" class="w-full bg-blue-600 text-white py-3 rounded-xl font-bold hover:bg-blue-700 transition-all shadow-lg shadow-blue-600/20 flex items-center justify-center gap-2">
                                        ${getIcon('plus', 'w-4 h-4')} Add to Cart
                                    </button>
                                </div>
                            </div>
                        `).join('')}
                    </div>
                `;
            } else if (title === 'My Orders') {
                container.innerHTML += `<div class="glass-card p-12 text-center rounded-xl fade-in"><p class="text-slate-500">Your order history will appear here after purchase.</p></div>`;
            }
        }

        function renderEmployeeView(title, container) {
            if(title === 'My Schedule') {
                container.innerHTML += `<div class="glass-card p-12 text-center rounded-xl fade-in"><p class="text-slate-500">No upcoming shifts scheduled.</p></div>`;
            }
        }

        function renderDriverView(title, container) {
            if(title === 'Active Delivery') {
                container.innerHTML += `<div class="glass-card p-12 text-center rounded-xl fade-in"><p class="text-slate-500">No active deliveries assigned.</p></div>`;
            }
        }

        // --- CART & USER MANAGEMENT LOGIC ---

        function addToCart(productId) {
            const product = STORE_PRODUCTS.find(p => p.id === productId);
            cart.push(product);
            
            // Update badge immediately
            const badge = document.querySelector('#content-area button span');
            if(badge) {
                badge.innerText = cart.length;
                badge.classList.remove('hidden');
            } else {
                // Re-render header if badge doesn't exist yet
                renderContent('E-Commerce Store');
            }
            
            // Show toast
            const toast = document.createElement('div');
            toast.className = 'fixed bottom-8 right-8 bg-slate-900 text-white px-6 py-3 rounded-xl shadow-2xl fade-in z-50 flex items-center gap-3';
            toast.innerHTML = `${getIcon('check-circle', 'w-5 h-5 text-emerald-400')} Added ${product.name} to cart`;
            document.body.appendChild(toast);
            setTimeout(() => toast.remove(), 2000);
        }

        function toggleCart() {
            if(cart.length === 0) {
                alert("Your cart is empty!");
                return;
            }
            const total = cart.reduce((sum, item) => sum + item.price, 0);
            alert(`🛒 Shopping Cart (${cart.length} items)\n\nTotal: $${total.toFixed(2)}\n\n[Checkout functionality would process payment here]`);
        }

        function banUser(userId) {
            const user = USERS_DB.find(u => u.id === userId);
            if(user) {
                user.status = 'banned';
                renderContent('User Management');
            }
        }

        function unbanUser(userId) {
            const user = USERS_DB.find(u => u.id === userId);
            if(user) {
                user.status = 'active';
                renderContent('User Management');
            }
        }

        // --- UI HELPERS ---
        function statCard(label, value, sub, color, icon) {
            const colors = { blue: 'text-blue-600 bg-blue-50', emerald: 'text-emerald-600 bg-emerald-50', red: 'text-red-600 bg-red-50', amber: 'text-amber-600 bg-amber-50' };
            return `
                <div class="glass-card p-6 rounded-xl flex items-center gap-4 hover:shadow-md transition-shadow">
                    <div class="w-12 h-12 ${colors[color]} rounded-lg flex items-center justify-center flex-shrink-0">${getIcon(icon, 'w-6 h-6')}</div>
                    <div>
                        <p class="text-xs font-bold text-slate-400 uppercase tracking-wider">${label}</p>
                        <p class="text-2xl font-bold text-slate-900 mt-1">${value}</p>
                        <p class="text-xs text-slate-500 mt-1">${sub}</p>
                    </div>
                </div>
            `;
        }

        function progressBar(label, val, color) {
            const barColor = color === 'green' ? 'bg-emerald-500' : color === 'amber' ? 'bg-amber-500' : 'bg-red-500';
            return `
                <div>
                    <div class="flex justify-between text-xs mb-1.5">
                        <span class="font-medium text-slate-600">${label}</span>
                        <span class="font-bold text-slate-800">${val}%</span>
                    </div>
                    <div class="w-full bg-slate-100 rounded-full h-2">
                        <div class="${barColor} h-2 rounded-full transition-all duration-1000" style="width: ${val}%"></div>
                    </div>
                </div>
            `;
        }

        function getIcon(name, sizeClass = 'w-5 h-5') {
            const icons = {
                'shield-check': '<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m5.618-4.016A11.955 11.955 0 0112 2.944a11.955 11.955 0 01-8.618 3.04A12.02 12.02 0 003 9c0 5.591 3.824 10.29 9 11.622 5.176-1.332 9-6.03 9-11.622 0-1.042-.133-2.052-.382-3.016z"/>',
                'users': '<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4.354a4 4 0 110 5.292M15 21H3v-1a6 6 0 0112 0v1zm0 0h6v-1a6 6 0 00-9-5.197M13 7a4 4 0 11-8 0 4 4 0 018 0z"/>',
                'shopping-bag': '<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 11V7a4 4 0 00-8 0v4M5 9h14l1 12H4L5 9z"/>',
                'calendar': '<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z"/>',
                'chat-alt-2': '<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 12h.01M12 12h.01M16 12h.01M21 12c0 4.418-4.03 8-9 8a9.863 9.863 0 01-4.255-.949L3 20l1.395-3.72C3.512 15.042 3 13.574 3 12c0-4.418 4.03-8 9-8s9 3.582 9 8z"/>',
                'check-circle': '<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"/>',
                'location-marker': '<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z"/><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z"/>',
                'document-text': '<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z"/>',
                'shopping-cart': '<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4 5M7 13l-2.293 2.293c-.63.63-.184 1.707.707 1.707H17m0 0a2 2 0 100 4 2 2 0 000-4zm-8 2a2 2 0 11-4 0 2 2 0 014 0z"/>',
                'currency-dollar': '<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z"/>',
                'ban': '<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M18.364 18.364A9 9 0 005.636 5.636m12.728 12.728A9 9 0 015.636 5.636m12.728 12.728L5.636 5.636"/>',
                'trash': '<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"/>',
                'plus': '<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4"/>'
            };
            return `<svg class="${sizeClass}" fill="none" stroke="currentColor" viewBox="0 0 24 24">${icons[name] || ''}</svg>`;
        }

        function toggleMobileMenu() {
            const sidebar = document.getElementById('sidebar');
            sidebar.classList.toggle('hidden');
            sidebar.classList.toggle('absolute');
            sidebar.classList.toggle('inset-0');
            sidebar.classList.toggle('w-full');
        }
    </script>
</body>
</html>


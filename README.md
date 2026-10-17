<!DOCTYPE html>
<html lang="ur" dir="ltr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NextLinks ISP Billing Management</title>
    <link rel="manifest" href="manifest.json">
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
</head>
<body class="bg-slate-900 text-slate-100 font-sans">

    <!-- Header / Navbar -->
    <header class="bg-slate-800 border-b border-slate-700 shadow-lg sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 py-4 flex flex-col md:flex-row justify-between items-center gap-4">
            <div class="flex items-center space-x-3 space-x-reverse">
                <div class="bg-emerald-600 p-2.5 rounded-xl text-white shadow-md">
                    <i class="fa-solid fa-network-wired text-xl"></i>
                </div>
                <div>
                    <h1 class="text-xl font-bold tracking-wide text-white">NextLinks | Billing Systems</h1>
                    <p class="text-xs text-slate-400">ISP Customer Management & WhatsApp Reminders</p>
                </div>
            </div>
            <div class="flex items-center gap-3">
                <span class="bg-emerald-500/10 text-emerald-400 text-xs px-3 py-1 rounded-full border border-emerald-500/20 font-medium">
                    <i class="fa-solid fa-circle text-[8px] mr-1.5 animate-pulse"></i> Active System
                </span>
                <button onclick="exportToCSV()" class="bg-slate-700 hover:bg-slate-600 text-slate-200 text-xs px-3 py-2 rounded-lg font-medium transition flex items-center gap-1.5">
                    <i class="fa-solid fa-download"></i> Export CSV
                </button>
            </div>
        </div>
    </header>

    <!-- Main Container -->
    <main class="max-w-7xl mx-auto px-4 py-6">

        <!-- Stats Overview Cards -->
        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 mb-6">
            <div class="bg-slate-800 border border-slate-700 p-4 rounded-xl shadow-sm">
                <div class="flex justify-between items-center">
                    <div>
                        <p class="text-xs text-slate-400 font-medium">Total Customers</p>
                        <h3 id="totalCount" class="text-2xl font-bold text-white mt-1">0</h3>
                    </div>
                    <div class="p-3 bg-blue-500/10 text-blue-400 rounded-lg">
                        <i class="fa-solid fa-users text-lg"></i>
                    </div>
                </div>
            </div>
            <div class="bg-slate-800 border border-slate-700 p-4 rounded-xl shadow-sm">
                <div class="flex justify-between items-center">
                    <div>
                        <p class="text-xs text-slate-400 font-medium">Total Balance / Dues</p>
                        <h3 id="totalBalance" class="text-2xl font-bold text-rose-400 mt-1">Rs. 0</h3>
                    </div>
                    <div class="p-3 bg-rose-500/10 text-rose-400 rounded-lg">
                        <i class="fa-solid fa-wallet text-lg"></i>
                    </div>
                </div>
            </div>
            <div class="bg-slate-800 border border-slate-700 p-4 rounded-xl shadow-sm">
                <div class="flex justify-between items-center">
                    <div>
                        <p class="text-xs text-slate-400 font-medium">Active Packages</p>
                        <h3 class="text-2xl font-bold text-emerald-400 mt-1">NWD 7MB / 11MB</h3>
                    </div>
                    <div class="p-3 bg-emerald-500/10 text-emerald-400 rounded-lg">
                        <i class="fa-solid fa-gauge-high text-lg"></i>
                    </div>
                </div>
            </div>
            <div class="bg-slate-800 border border-slate-700 p-4 rounded-xl shadow-sm">
                <div class="flex justify-between items-center">
                    <div>
                        <p class="text-xs text-slate-400 font-medium">Sellers / Admins</p>
                        <h3 class="text-2xl font-bold text-amber-400 mt-1">Babar / Riaz</h3>
                    </div>
                    <div class="p-3 bg-amber-500/10 text-amber-400 rounded-lg">
                        <i class="fa-solid fa-user-shield text-lg"></i>
                    </div>
                </div>
            </div>
        </div>

        <!-- Add New Customer Section -->
        <div class="bg-slate-800 border border-slate-700 p-5 rounded-xl shadow-sm mb-6">
            <h3 class="text-md font-bold text-white mb-3 flex items-center gap-2">
                <i class="fa-solid fa-user-plus text-emerald-400"></i> نیا کسٹمر شامل کریں (Add New Customer)
            </h3>
            <form id="addCustomerForm" onsubmit="addCustomer(event)" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-6 gap-3">
                <input type="text" id="newName" placeholder="کسٹمر کا نام ● یوزر نیم" required
                    class="bg-slate-900 border border-slate-700 rounded-lg px-3 py-2 text-sm text-white focus:outline-none focus:border-emerald-500">
                <select id="newPackage" class="bg-slate-900 border border-slate-700 text-slate-300 text-sm rounded-lg px-3 py-2 focus:outline-none focus:border-emerald-500">
                    <option value="NWD-7MB">NWD-7MB</option>
                    <option value="NWD-11MB">NWD-11MB</option>
                    <option value="NWD-17MB">NWD-17MB</option>
                </select>
                <input type="text" id="newSeller" placeholder="سیلر (مثلاً Babar01)" required
                    class="bg-slate-900 border border-slate-700 rounded-lg px-3 py-2 text-sm text-white focus:outline-none focus:border-emerald-500">
                <input type="text" id="newBalance" placeholder="بیلنس (مثلاً -2,400.00)" required
                    class="bg-slate-900 border border-slate-700 rounded-lg px-3 py-2 text-sm text-white focus:outline-none focus:border-emerald-500">
                <input type="text" id="newExpiry" placeholder="ایکسپائری (29 Oct 2026)" required
                    class="bg-slate-900 border border-slate-700 rounded-lg px-3 py-2 text-sm text-white focus:outline-none focus:border-emerald-500">
                <button type="submit" class="bg-emerald-600 hover:bg-emerald-500 text-white font-medium text-sm px-4 py-2 rounded-lg transition shadow-sm flex items-center justify-center gap-1.5">
                    <i class="fa-solid fa-plus"></i> محفوظ کریں
                </button>
            </form>
        </div>

        <!-- Controls & Search -->
        <div class="bg-slate-800 border border-slate-700 p-4 rounded-xl shadow-sm mb-6 flex flex-col md:flex-row justify-between items-center gap-4">
            <div class="w-full md:w-96 relative">
                <span class="absolute inset-y-0 left-0 flex items-center pl-3 text-slate-400">
                    <i class="fa-solid fa-search"></i>
                </span>
                <input type="text" id="searchInput" onkeyup="filterTable()" placeholder="ਕسٹمر کا نام یا یوزر نیم تلاش کریں..." 
                    class="w-full bg-slate-900 border border-slate-700 rounded-lg pl-10 pr-4 py-2 text-sm text-white focus:outline-none focus:border-emerald-500 transition">
            </div>
            <div class="flex items-center gap-3 w-full md:w-auto justify-end">
                <select id="sellerFilter" onchange="filterTable()" class="bg-slate-900 border border-slate-700 text-slate-300 text-sm rounded-lg px-3 py-2 focus:outline-none focus:border-emerald-500">
                    <option value="">تمام سیلرز (All Sellers)</option>
                    <option value="Babar">Babar / Babar01</option>
                    <option value="Riaz">Riaz01</option>
                </select>
            </div>
        </div>

        <!-- Customers Table -->
        <div class="bg-slate-800 border border-slate-700 rounded-xl shadow-sm overflow-hidden">
            <div class="overflow-x-auto">
                <table id="customerTable" class="w-full text-left border-collapse">
                    <thead>
                        <tr class="bg-slate-900/60 border-b border-slate-700 text-xs font-semibold text-slate-400 uppercase tracking-wider">
                            <th class="py-3.5 px-4">Name / Username</th>
                            <th class="py-3.5 px-4">Package</th>
                            <th class="py-3.5 px-4">Seller</th>
                            <th class="py-3.5 px-4">Balance</th>
                            <th class="py-3.5 px-4">Expiry Date</th>
                            <th class="py-3.5 px-4 text-center">WhatsApp Reminder</th>
                        </tr>
                    </thead>
                    <tbody id="tableBody" class="divide-y divide-slate-700 text-sm text-slate-300">
                        <!-- Data populated via JS -->
                    </tbody>
                </table>
            </div>
        </div>

    </main>

    <!-- JavaScript Application Logic -->
    <script>
        let customersData = JSON.parse(localStorage.getItem('isp_customers')) || [
            { name: "Shahid hussian ● rbs057", package: "NWD-11MB", seller: "Babar01", balance: "-2,700.00", expiry: "29 Oct 2026 12:00" },
            { name: "Farhan Hyder ● rior40", package: "NWD-7MB", seller: "Riaz01", balance: "-2,400.00", expiry: "29 Oct 2026 12:00" },
            { name: "Muhammad eshaque magsi ● rbs118", package: "NWD-7MB", seller: "Babar01", balance: "-2,400.00", expiry: "29 Oct 2026 12:00" },
            { name: "Ghulam Mujtaba ● rior244", package: "NWD-7MB", seller: "Riaz01", balance: "-2,400.00", expiry: "29 Oct 2026 12:00" },
            { name: "Javeed Akhtar ● rio94", package: "NWD-11MB", seller: "babar", balance: "-2,000.00", expiry: "29 Oct 2026 12:00" },
            { name: "Waheed Ali Shaikh ● rior60", package: "NWD-7MB", seller: "Riaz01", balance: "-2,400.00", expiry: "29 Oct 2026 12:00" },
            { name: "Zeeshan Usman ● rio69", package: "NWD-11MB", seller: "babar", balance: "-2,000.00", expiry: "29 Oct 2026 12:00" },
            { name: "Muhammad Naeem ● rior266", package: "NWD-7MB", seller: "Riaz01", balance: "-2,400.00", expiry: "29 Oct 2026 12:00" },
            { name: "Shamasuddin Khokhar ● rio30", package: "NWD-11MB", seller: "babar", balance: "-2,000.00", expiry: "29 Oct 2026 12:00" },
            { name: "MuhammadAliperzado ● rbs111", package: "NWD-11MB", seller: "Babar01", balance: "-2,700.00", expiry: "29 Oct 2026 12:00" }
        ];

        function renderTable(data) {
            const tbody = document.getElementById('tableBody');
            tbody.innerHTML = '';
            
            document.getElementById('totalCount').innerText = data.length;

            let totalBal = 0;
            data.forEach((item, index) => {
                let numericBal = parseFloat(item.balance.replace(/,/g, '').replace(/-/g, '')) || 0;
                // Accumulate balance display safely
                totalBal -= numericBal;

                const tr = document.createElement('tr');
                tr.className = "hover:bg-slate-700/50 transition";
                
                let message = encodeURIComponent(`محترم صارف (${item.name})، آپ کے نیکسٹ لنکس انٹرنیٹ پیکج (${item.package}) کا بل بقایا جات (${item.balance}) کے ساتھ قابلِ ادائیگی ہے۔ برائے مہربانی تاریخ اخراج (${item.expiry}) سے پہلے بل ادا کریں شکریہ۔`);
                
                tr.innerHTML = `
                    <td class="py-3 px-4 font-medium text-white">${item.name}</td>
                    <td class="py-3 px-4"><span class="bg-blue-500/10 text-blue-400 text-xs px-2.5 py-1 rounded-md border border-blue-500/20">${item.package}</span></td>
                    <td class="py-3 px-4 text-slate-400">${item.seller}</td>
                    <td class="py-3 px-4 font-semibold text-rose-400">${item.balance}</td>
                    <td class="py-3 px-4 text-slate-400 text-xs">${item.expiry}</td>
                    <td class="py-3 px-4 text-center flex items-center justify-center gap-2">
                        <a href="https://wa.me/?text=${message}" target="_blank" 
                           class="inline-flex items-center gap-1.5 bg-emerald-600 hover:bg-emerald-500 text-white text-xs px-3 py-1.5 rounded-lg font-medium transition shadow-sm">
                            <i class="fa-brands fa-whatsapp text-sm"></i> ریمائنڈر
                        </a>
                        <button onclick="deleteCustomer(${index})" class="text-rose-400 hover:text-rose-300 px-2 py-1 text-xs" title="حذف کریں">
                            <i class="fa-solid fa-trash"></i>
                        </button>
                    </td>
                `;
                tbody.appendChild(tr);
            });

            document.getElementById('totalBalance').innerText = "Rs. " + totalBal.toLocaleString();
        }

        function addCustomer(event) {
            event.preventDefault();
            const name = document.getElementById('newName').value;
            const pkg = document.getElementById('newPackage').value;
            const seller = document.getElementById('newSeller').value;
            const balance = document.getElementById('newBalance').value;
            const expiry = document.getElementById('newExpiry').value;

            customersData.unshift({ name, package: pkg, seller, balance, expiry });
            localStorage.setItem('isp_customers', JSON.stringify(customersData));
            
            renderTable(customersData);
            document.getElementById('addCustomerForm').reset();
        }

        function deleteCustomer(index) {
            if(confirm("کیا آپ واقعی اس کسٹمر کو ڈیلیٹ کرنا چاہتے ہیں؟")) {
                customersData.splice(index, 1);
                localStorage.setItem('isp_customers', JSON.stringify(customersData));
                renderTable(customersData);
            }
        }

        function filterTable() {
            const searchVal = document.getElementById('searchInput').value.toLowerCase();
            const sellerVal = document.getElementById('sellerFilter').value.toLowerCase();

            const filtered = customersData.filter(item => {
                const matchSearch = item.name.toLowerCase().includes(searchVal) || item.package.toLowerCase().includes(searchVal);
                const matchSeller = sellerVal === "" || item.seller.toLowerCase().includes(sellerVal);
                return matchSearch && matchSeller;
            });

            renderTable(filtered);
        }

        function exportToCSV() {
            let csvContent = "Name / Username,Package,Seller,Balance,Expiry\n";
            customersData.forEach(row => {
                csvContent += `"${row.name}","${row.package}","${row.seller}","${row.balance}","${row.expiry}"\n`;
            });
            const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' });
            const link = document.createElement("a");
            link.href = URL.createObjectURL(blob);
            link.download = "NextLinks_Billing_Export.csv";
            link.click();
        }

        // Initial Load
        renderTable(customersData);
    </script>
</body>
</html>

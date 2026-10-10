# calc.github.io
m-o-m budget projections

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Personal Finance Utility Calculators</title>
    <!-- Tailwind CSS for clean styling -->
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
</head>
<body class="bg-slate-50 text-slate-800 font-sans p-6 max-w-4xl mx-auto">

    <header class="mb-8 text-center">
        <h1 class="text-3xl font-bold text-slate-900">Personal Finance Utilities</h1>
        <p class="text-slate-600 mt-1">Lightweight local calculators for budgeting and debt payoff.</p>
    </header>

    <!-- CALCULATOR 1: BUDGET & SURPLUS PLANNER -->
    <section class="bg-white p-6 rounded-2xl shadow-sm border border-slate-200 mb-8">
        <h2 class="text-xl font-semibold mb-4 text-blue-600">1. Monthly Surplus & Budget Planner</h2>
        
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <!-- Inputs -->
            <div class="space-y-4">
                <div>
                    <label class="block text-sm font-medium text-slate-700">Net Monthly Take-Home Pay ($)</label>
                    <input type="number" id="income" value="2800" class="mt-1 w-full p-2 border border-slate-300 rounded-lg" oninput="calculateBudget()">
                </div>
                <div>
                    <label class="block text-sm font-medium text-slate-700">Rent & Utilities ($)</label>
                    <input type="range" id="rent" min="300" max="2000" step="50" value="1100" class="w-full accent-blue-600" oninput="updateVal('rent', 'rentVal'); calculateBudget()">
                    <span id="rentVal" class="text-sm text-slate-500">$1,100</span>
                </div>
                <div>
                    <label class="block text-sm font-medium text-slate-700">Groceries & Essentials ($)</label>
                    <input type="range" id="groceries" min="100" max="800" step="25" value="350" class="w-full accent-blue-600" oninput="updateVal('groceries', 'groceriesVal'); calculateBudget()">
                    <span id="groceriesVal" class="text-sm text-slate-500">$350</span>
                </div>
                <div>
                    <label class="block text-sm font-medium text-slate-700">Transit / Transport ($)</label>
                    <input type="range" id="transport" min="0" max="600" step="25" value="200" class="w-full accent-blue-600" oninput="updateVal('transport', 'transportVal'); calculateBudget()">
                    <span id="transportVal" class="text-sm text-slate-500">$200</span>
                </div>
                <div>
                    <label class="block text-sm font-medium text-slate-700">Discretionary / Wants ($)</label>
                    <input type="range" id="wants" min="0" max="800" step="25" value="300" class="w-full accent-blue-600" oninput="updateVal('wants', 'wantsVal'); calculateBudget()">
                    <span id="wantsVal" class="text-sm text-slate-500">$300</span>
                </div>
            </div>

            <!-- Outputs -->
            <div class="bg-blue-50 p-6 rounded-xl flex flex-col justify-center items-center text-center">
                <span class="text-sm font-semibold uppercase tracking-wider text-blue-800">Monthly Surplus (Savings / Investing)</span>
                <span id="surplusDisplay" class="text-4xl font-bold text-blue-900 mt-2">$850</span>
                <span id="savingsRate" class="text-sm text-blue-700 mt-1">30.4% Savings Rate</span>
            </div>
        </div>
    </section>

    <!-- CALCULATOR 2: DEBT PAYOFF TIMELINE -->
    <section class="bg-white p-6 rounded-2xl shadow-sm border border-slate-200">
        <h2 class="text-xl font-semibold mb-4 text-emerald-600">2. Debt Payoff Timeline Simulation</h2>
        
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <div class="space-y-4">
                <div>
                    <label class="block text-sm font-medium text-slate-700">Total Starting Debt ($)</label>
                    <input type="number" id="totalDebt" value="4000" class="mt-1 w-full p-2 border border-slate-300 rounded-lg" oninput="calculateDebt()">
                </div>
                <div>
                    <label class="block text-sm font-medium text-slate-700">Monthly Debt Payment Allocation ($)</label>
                    <input type="range" id="monthlyPayment" min="100" max="1500" step="50" value="500" class="w-full accent-emerald-600" oninput="updateVal('monthlyPayment', 'paymentVal'); calculateDebt()">
                    <span id="paymentVal" class="text-sm text-slate-500">$500</span>
                </div>
            </div>

            <div class="bg-emerald-50 p-6 rounded-xl flex flex-col justify-center items-center text-center">
                <span class="text-sm font-semibold uppercase tracking-wider text-emerald-800">Estimated Payoff Timeline</span>
                <span id="payoffDisplay" class="text-4xl font-bold text-emerald-900 mt-2">8.0 Months</span>
                <span id="interestPaid" class="text-sm text-emerald-700 mt-1">Total Debt Eliminated</span>
            </div>
        </div>
    </section>

    <!-- JavaScript Logic -->
    <script>
        function updateVal(sliderId, labelId) {
            document.getElementById(labelId).innerText = "$" + Number(document.getElementById(sliderId).value).toLocaleString();
        }

        function calculateBudget() {
            let income = parseFloat(document.getElementById('income').value) || 0;
            let rent = parseFloat(document.getElementById('rent').value) || 0;
            let groceries = parseFloat(document.getElementById('groceries').value) || 0;
            let transport = parseFloat(document.getElementById('transport').value) || 0;
            let wants = parseFloat(document.getElementById('wants').value) || 0;

            let totalExpenses = rent + groceries + transport + wants;
            let surplus = income - totalExpenses;
            let rate = income > 0 ? ((surplus / income) * 100).toFixed(1) : 0;

            let display = document.getElementById('surplusDisplay');
            display.innerText = (surplus < 0 ? "-$" : "$") + Math.abs(surplus).toLocaleString();
            display.className = surplus >= 0 ? "text-4xl font-bold text-blue-900 mt-2" : "text-4xl font-bold text-red-600 mt-2";
            
            document.getElementById('savingsRate').innerText = rate + "% Savings Rate";
        }

        function calculateDebt() {
            let debt = parseFloat(document.getElementById('totalDebt').value) || 0;
            let payment = parseFloat(document.getElementById('monthlyPayment').value) || 1;

            if (payment <= 0 || debt <= 0) {
                document.getElementById('payoffDisplay').innerText = "N/A";
                return;
            }

            // Simple linear payoff estimate (assuming blended or low interest integration)
            let months = debt / payment;
            let display = document.getElementById('payoffDisplay');
            
            display.innerText = months.toFixed(1) + " Months (~" + (months / 12).toFixed(1) + " Years)";
        }

        // Run on load
        calculateBudget();
        calculateDebt();
    </script>
</body>
</html>


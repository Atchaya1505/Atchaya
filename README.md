<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Credentially Onboarding Prototype</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Custom styles for Inter font and general body styling */
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f0f4f8; /* Light gray background */
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
            border-radius: 1rem; /* Rounded corners for the main container */
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
            padding: 2.5rem; /* Increased padding */
            max-width: 900px; /* Increased max-width */
            width: 100%;
            text-align: center;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 500px; /* Minimum height for better visual */
        }
        .page {
            display: none; /* Hidden by default */
            width: 100%;
            animation: fadeIn 0.5s ease-in-out; /* Fade-in animation */
        }
        .page.active {
            display: block; /* Active page is shown */
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        input[type="text"],
        input[type="email"],
        input[type="tel"],
        input[type="number"] {
            padding: 0.75rem 1rem;
            border: 1px solid #cbd5e1; /* Light gray border */
            border-radius: 0.5rem; /* Rounded corners */
            width: 100%;
            box-sizing: border-box;
            transition: border-color 0.2s ease-in-out;
        }
        input[type="text"]:focus,
        input[type="email"]:focus,
        input[type="tel"]:focus,
        input[type="number"]:focus {
            outline: none;
            border-color: #6366f1; /* Indigo on focus */
            box-shadow: 0 0 0 3px rgba(99, 102, 241, 0.2); /* Light indigo shadow on focus */
        }
        .btn-primary {
            background-color: #6366f1; /* Indigo */
            color: white;
            padding: 0.75rem 2rem;
            border-radius: 0.75rem; /* More rounded */
            font-weight: 600;
            transition: background-color 0.2s ease-in-out, transform 0.1s ease-in-out;
            box-shadow: 0 4px 10px rgba(99, 102, 241, 0.3); /* Soft shadow */
        }
        .btn-primary:hover {
            background-color: #4f46e5; /* Darker indigo on hover */
            transform: translateY(-2px); /* Slight lift effect */
        }
        .btn-secondary {
            background-color: #e2e8f0; /* Light gray */
            color: #475569; /* Slate gray text */
            padding: 0.75rem 2rem;
            border-radius: 0.75rem;
            font-weight: 600;
            transition: background-color 0.2s ease-in-out, transform 0.1s ease-in-out;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.05);
        }
        .btn-secondary:hover {
            background-color: #cbd5e1; /* Darker light gray on hover */
            transform: translateY(-2px);
        }
        .checkbox-container {
            display: flex;
            align-items: center;
            margin-bottom: 0.75rem;
            cursor: pointer;
            user-select: none; /* Prevent text selection on double click */
        }
        .checkbox-container input[type="checkbox"] {
            appearance: none;
            -webkit-appearance: none;
            width: 1.25rem;
            height: 1.25rem;
            border: 2px solid #94a3b8; /* Gray border */
            border-radius: 0.375rem; /* Rounded corners */
            margin-right: 0.75rem;
            cursor: pointer;
            position: relative;
            transition: background-color 0.2s, border-color 0.2s;
        }
        .checkbox-container input[type="checkbox"]:checked {
            background-color: #6366f1; /* Indigo when checked */
            border-color: #6366f1;
        }
        .checkbox-container input[type="checkbox"]:checked::after {
            content: '';
            position: absolute;
            top: 3px;
            left: 3px;
            width: 12px;
            height: 6px;
            border: 2px solid white;
            border-top: none;
            border-right: none;
            transform: rotate(-45deg);
        }
        .modal {
            display: none; /* Hidden by default */
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto;
            background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
            justify-content: center;
            align-items: center;
        }
        .modal-content {
            background-color: #fefefe;
            margin: auto;
            padding: 2rem;
            border-radius: 1rem;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
            width: 80%;
            max-width: 500px;
            text-align: center;
            position: relative;
            animation: slideIn 0.3s ease-out;
        }
        @keyframes slideIn {
            from { transform: translateY(-50px); opacity: 0; }
            to { transform: translateY(0); opacity: 1; }
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
        .loading-spinner {
            border: 4px solid rgba(0, 0, 0, 0.1);
            border-left-color: #6366f1; /* Match primary button color */
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 1s linear infinite;
            margin: 20px auto;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Page 1: Landing Page -->
        <div id="landing-page" class="page active">
            <h1 class="text-4xl font-extrabold text-gray-900 mb-6">Welcome to Credentially</h1>
            <p class="text-lg text-gray-700 mb-8 max-w-2xl">Streamline your credentialing, compliance, and onboarding processes with our AI-powered platform. Get started with a personalized journey tailored to your organization's needs.</p>
            <button id="try-me-btn" class="btn-primary text-xl px-8 py-4">Try Me</button>
        </div>

        <!-- Page 2: Collect Info Page -->
        <div id="collect-info-page" class="page">
            <h2 class="text-3xl font-bold text-gray-900 mb-6">Tell Us About Your Organization</h2>
            <p class="text-gray-600 mb-8">Just a few details to begin your personalized Credentially onboarding.</p>
            <form id="info-form" class="w-full max-w-md mx-auto">
                <div class="mb-5 text-left">
                    <label for="name" class="block text-gray-700 text-sm font-medium mb-2">Your Full Name</label>
                    <input type="text" id="name" name="name" placeholder="Jane Smith" required class="w-full">
                </div>
                <div class="mb-5 text-left">
                    <label for="org-name" class="block text-gray-700 text-sm font-medium mb-2">Organization Name</label>
                    <input type="text" id="org-name" name="org-name" placeholder="Acme Healthcare" required class="w-full">
                </div>
                <div class="mb-5 text-left">
                    <label for="email" class="block text-gray-700 text-sm font-medium mb-2">Work Email Address</label>
                    <input type="email" id="email" name="email" placeholder="jane.smith@acme.com" required class="w-full">
                </div>
                <div class="mb-8 text-left">
                    <label for="phone" class="block text-gray-700 text-sm font-medium mb-2">Phone Number</label>
                    <input type="tel" id="phone" name="phone" placeholder="+1234567890" required class="w-full">
                </div>
                <button type="submit" class="btn-primary w-full">Submit & Verify</button>
            </form>
        </div>

        <!-- Page 3: Gather Requirements Page -->
        <div id="gather-requirements-page" class="page">
            <h2 class="text-3xl font-bold text-gray-900 mb-6">Your Credentialing Needs</h2>
            <p class="text-gray-600 mb-8">Help us understand your specific credentialing and compliance requirements. Our AI will use this to tailor your solution.</p>
            <form id="requirements-form" class="w-full max-w-xl mx-auto text-left">
                <div class="mb-6">
                    <h3 class="text-xl font-semibold text-gray-800 mb-4">Key Credentialing Areas:</h3>
                    <label class="checkbox-container">
                        <input type="checkbox" name="requirements" value="license-verification">
                        Automate License & Certification Verification
                    </label>
                    <label class="checkbox-container">
                        <input type="checkbox" name="background-checks">
                        Streamline Background Checks & Screenings
                    </label>
                    <label class="checkbox-container">
                        <input type="checkbox" name="compliance-tracking">
                        Ensure Ongoing Compliance & Regulatory Tracking
                    </label>
                    <label class="checkbox-container">
                        <input type="checkbox" name="document-management">
                        Centralize Document Management for Credentials
                    </label>
                    <label class="checkbox-container">
                        <input type="checkbox" name="onboarding-workflows">
                        Automate Onboarding Workflows for New Hires/Practitioners
                    </label>
                    <label class="checkbox-container">
                        <input type="checkbox" name="provider-enrollment">
                        Expedite Provider Enrollment & Payer Credentialing
                    </label>
                </div>
                <div class="mb-8">
                    <h3 class="text-xl font-semibold text-gray-800 mb-4">Desired Level of AI Assistance:</h3>
                    <label class="checkbox-container">
                        <input type="checkbox" name="ai-level" value="basic-ai">
                        Basic AI (e.g., automated reminders, simple data validation)
                    </label>
                    <label class="checkbox-container">
                        <input type="checkbox" name="advanced-ai">
                        Advanced AI (e.g., intelligent document parsing, fraud detection)
                    </label>
                    <label class="checkbox-container">
                        <input type="checkbox" name="full-ai">
                        Comprehensive AI (predictive compliance, automated decision support)
                    </label>
                </div>
                <button type="submit" class="btn-primary w-full">Submit Requirements</button>
            </form>
        </div>

        <!-- Page 4: Subscription Packages Page -->
        <div id="subscription-packages-page" class="page">
            <h2 class="text-3xl font-bold text-gray-900 mb-6">Your Recommended Credentialing Plans</h2>
            <p class="text-gray-600 mb-8">Based on your organization's needs, our AI recommends the following plans:</p>

            <div class="grid grid-cols-1 md:grid-cols-3 gap-8 w-full max-w-4xl mx-auto">
                <!-- Starter Credentialing Package -->
                <div class="bg-gray-50 p-6 rounded-xl shadow-md border-2 border-transparent hover:border-indigo-500 transition-all duration-300">
                    <h3 class="text-2xl font-bold text-gray-900 mb-4">Starter Credentialing</h3>
                    <p class="text-gray-600 mb-4">Ideal for small practices or teams managing basic credentialing.</p>
                    <p class="text-4xl font-extrabold text-indigo-600 mb-6">$99<span class="text-lg font-medium text-gray-500">/month</span></p>
                    <ul class="text-left text-gray-700 mb-8 space-y-2">
                        <li>&#10003; 50 Credential Profiles</li>
                        <li>&#10003; Basic License Verification</li>
                        <li>&#10003; Standard Document Storage</li>
                        <li>&#10003; Email Support</li>
                    </ul>
                    <button class="btn-primary w-full start-trial-btn" data-package="Starter Credentialing">Start 7-Day Trial</button>
                </div>

                <!-- Pro Compliance Package (Recommended) -->
                <div class="bg-indigo-50 p-6 rounded-xl shadow-lg border-2 border-indigo-600 relative">
                    <span class="absolute top-0 right-0 -mt-4 -mr-4 bg-indigo-600 text-white text-xs font-bold px-3 py-1 rounded-full shadow-md">Recommended by AI</span>
                    <h3 class="text-2xl font-bold text-indigo-800 mb-4">Pro Compliance</h3>
                    <p class="text-indigo-700 mb-4">Perfect for growing organizations needing robust compliance tools.</p>
                    <p class="text-4xl font-extrabold text-indigo-700 mb-6">$299<span class="text-lg font-medium text-indigo-500">/month</span></p>
                    <ul class="text-left text-indigo-800 mb-8 space-y-2">
                        <li>&#10003; 250 Credential Profiles</li>
                        <li>&#10003; Advanced License & Certification Verification</li>
                        <li>&#10003; Automated Background Checks</li>
                        <li>&#10003; AI-Powered Document Parsing</li>
                        <li>&#10003; Priority Support</li>
                    </ul>
                    <button class="btn-primary w-full start-trial-btn" data-package="Pro Compliance">Start 7-Day Trial</button>
                    <button class="btn-secondary w-full mt-4 sign-up-now-btn" data-package="Pro Compliance">Sign Up Now</button>
                </div>

                <!-- Enterprise Credentialing Suite Package -->
                <div class="bg-gray-50 p-6 rounded-xl shadow-md border-2 border-transparent hover:border-indigo-500 transition-all duration-300">
                    <h3 class="text-2xl font-bold text-gray-900 mb-4">Enterprise Suite</h3>
                    <p class="text-gray-600 mb-4">For large enterprises requiring comprehensive, custom credentialing solutions.</p>
                    <p class="text-4xl font-extrabold text-indigo-600 mb-6">$799<span class="text-lg font-medium text-gray-500">/month</span></p>
                    <ul class="text-left text-gray-700 mb-8 space-y-2">
                        <li>&#10003; Unlimited Credential Profiles</li>
                        <li>&#10003; Full AI Suite for Compliance & Fraud Detection</li>
                        <li>&#10003; Dedicated Account Manager</li>
                        <li>&#10003; Custom Integrations & API Access</li>
                        <li>&#10003; Personal Setup & Training</li>
                    </ul>
                    <button class="btn-primary w-full start-trial-btn" data-package="Enterprise Suite">Start 7-Day Trial</button>
                </div>
            </div>
            <p class="text-sm text-gray-500 mt-8">You can play around during the 7-day trial period and sign up for a full subscription anytime.</p>
        </div>

        <!-- Page 4.5: Collect Account Details Page -->
        <div id="collect-account-details-page" class="page">
            <h2 class="text-3xl font-bold text-gray-900 mb-6">Provide Your Billing Details</h2>
            <p class="text-gray-600 mb-8">To finalize your subscription, please provide your organization's billing information.</p>
            <form id="account-details-form" class="w-full max-w-md mx-auto">
                <div class="mb-5 text-left">
                    <label for="bank-name" class="block text-gray-700 text-sm font-medium mb-2">Bank Name</label>
                    <input type="text" id="bank-name" name="bank-name" placeholder="e.g., Bank of Credentialing" required class="w-full">
                </div>
                <div class="mb-5 text-left">
                    <label for="account-holder-name" class="block text-gray-700 text-sm font-medium mb-2">Account Holder Name</label>
                    <input type="text" id="account-holder-name" name="account-holder-name" placeholder="Acme Healthcare Inc." required class="w-full">
                </div>
                <div class="mb-5 text-left">
                    <label for="account-number" class="block text-gray-700 text-sm font-medium mb-2">Account Number</label>
                    <input type="number" id="account-number" name="account-number" placeholder="1234567890" required class="w-full">
                </div>
                <div class="mb-8 text-left">
                    <label for="routing-number" class="block text-gray-700 text-sm font-medium mb-2">Routing Number</label>
                    <input type="number" id="routing-number" name="routing-number" placeholder="098765432" required class="w-full">
                </div>
                <button type="submit" class="btn-primary w-full">Submit Billing Details</button>
            </form>
        </div>

        <!-- Page 5: Estimate & Payment Page -->
        <div id="estimate-payment-page" class="page">
            <h2 class="text-3xl font-bold text-gray-900 mb-6">Your Credentially Subscription Estimate</h2>
            <div id="estimate-details" class="bg-white p-6 rounded-xl shadow-md mb-8 w-full max-w-md mx-auto text-left">
                <p class="text-lg font-semibold text-gray-800 mb-4">Plan: <span id="estimate-package" class="font-normal text-indigo-600"></span></p>
                <p class="text-lg font-semibold text-gray-800 mb-4">Monthly Cost: <span id="estimate-cost" class="font-normal text-indigo-600"></span></p>
                <p class="text-lg font-semibold text-gray-800 mb-4">Key Features:</p>
                <ul id="estimate-features" class="list-disc list-inside text-gray-700 ml-4">
                    </ul>
                <p class="text-2xl font-bold text-gray-900 mt-6">Total Due: <span id="estimate-total" class="text-indigo-700"></span></p>
            </div>
            <button id="proceed-payment-btn" class="btn-primary w-full max-w-md">Proceed to Payment</button>
        </div>

        <!-- Page 6: Contract & Setup Page -->
        <div id="contract-setup-page" class="page">
            <h2 class="text-3xl font-bold text-gray-900 mb-6">Welcome to Credentially!</h2>
            <p class="text-lg text-gray-700 mb-4">Your payment has been approved, and your Credentially account is now fully active!</p>
            <p class="text-lg text-gray-700 mb-8">A contract has been sent to your email for digital signature and record-keeping. Please check your inbox.</p>
            <div id="personal-assistance-message" class="bg-blue-50 p-4 rounded-lg text-blue-800 text-md mb-8 hidden">
                <p class="font-semibold mb-2">Personal Setup & Training Included!</p>
                <p>As per your chosen plan, a personal setup and training session is included. Our team will contact you shortly to schedule your personalized onboarding session.</p>
            </div>
            <button id="go-to-dashboard-btn" class="btn-primary">Go to Credentially Dashboard</button>
        </div>

        <!-- Modal for messages -->
        <div id="message-modal" class="modal">
            <div class="modal-content">
                <span class="close-button">&times;</span>
                <div id="modal-spinner" class="loading-spinner hidden"></div>
                <p id="modal-message" class="text-lg text-gray-800 mb-4"></p>
                <button id="modal-ok-btn" class="btn-primary px-6 py-2">OK</button>
            </div>
        </div>

    </div>

    <script>
        // Global state to manage pages and user data
        const pages = document.querySelectorAll('.page');
        let currentPageIndex = 0;
        let userData = {};
        let selectedPackage = null;

        // Get references to elements
        const landingPage = document.getElementById('landing-page');
        const tryMeBtn = document.getElementById('try-me-btn');
        const collectInfoPage = document.getElementById('collect-info-page');
        const infoForm = document.getElementById('info-form');
        const gatherRequirementsPage = document.getElementById('gather-requirements-page');
        const requirementsForm = document.getElementById('requirements-form');
        const subscriptionPackagesPage = document.getElementById('subscription-packages-page');
        const collectAccountDetailsPage = document.getElementById('collect-account-details-page');
        const accountDetailsForm = document.getElementById('account-details-form');
        const estimatePaymentPage = document.getElementById('estimate-payment-page');
        const proceedPaymentBtn = document.getElementById('proceed-payment-btn');
        const contractSetupPage = document.getElementById('contract-setup-page');
        const goToDashboardBtn = document.getElementById('go-to-dashboard-btn');
        const personalAssistanceMessage = document.getElementById('personal-assistance-message');

        const messageModal = document.getElementById('message-modal');
        const modalMessage = document.getElementById('modal-message');
        const modalOkBtn = document.getElementById('modal-ok-btn');
        const modalCloseBtn = document.querySelector('.close-button');
        const modalSpinner = document.getElementById('modal-spinner');

        // Define package details for dynamic display, adapted for Credentially
        const packageDetails = {
            "Starter Credentialing": {
                cost: 99,
                features: ["50 Credential Profiles", "Basic License Verification", "Standard Document Storage", "Email Support"],
                personalAssistance: false
            },
            "Pro Compliance": {
                cost: 299,
                features: ["250 Credential Profiles", "Advanced License & Certification Verification", "Automated Background Checks", "AI-Powered Document Parsing", "Priority Support"],
                personalAssistance: false
            },
            "Enterprise Suite": {
                cost: 799,
                features: ["Unlimited Credential Profiles", "Full AI Suite for Compliance & Fraud Detection", "Dedicated Account Manager", "Custom Integrations & API Access", "Personal Setup & Training"],
                personalAssistance: true
            }
        };

        // Function to show a specific page
        function showPage(index) {
            pages.forEach((page, i) => {
                if (i === index) {
                    page.classList.add('active');
                } else {
                    page.classList.remove('active');
                }
            });
            currentPageIndex = index;
        }

        // Function to show the modal with a message and optional spinner
        function showModal(message, showSpinner = false) {
            modalMessage.textContent = message;
            if (showSpinner) {
                modalSpinner.classList.remove('hidden');
                modalOkBtn.classList.add('hidden'); // Hide OK button when spinner is shown
            } else {
                modalSpinner.classList.add('hidden');
                modalOkBtn.classList.remove('hidden'); // Show OK button when spinner is hidden
            }
            messageModal.style.display = 'flex'; // Use flex to center content
        }

        // Function to hide the modal
        function hideModal() {
            messageModal.style.display = 'none';
        }

        // Event Listeners

        // Landing page "Try Me" button
        tryMeBtn.addEventListener('click', () => {
            showPage(1); // Go to Collect Info Page
        });

        // Collect Info Form submission
        infoForm.addEventListener('submit', async (e) => {
            e.preventDefault();
            const formData = new FormData(infoForm);
            userData = Object.fromEntries(formData.entries());

            showModal("Verifying your organization's details...", true);

            // Simulate API call for verification
            await new Promise(resolve => setTimeout(resolve, 2000)); // Simulate network delay

            // Call LLM for verification (e.g., check if organization name sounds plausible)
            const prompt = `Verify the following organization details for plausibility. Respond with 'true' if plausible, 'false' otherwise.
            Name: ${userData.name}
            Organization Name: ${userData['org-name']}
            Email: ${userData.email}
            Phone: ${userData.phone}
            Consider if the email matches the organization name format.`;

            const payload = {
                contents: [{ role: "user", parts: [{ text: prompt }] }],
                generationConfig: {
                    responseMimeType: "application/json",
                    responseSchema: {
                        type: "OBJECT",
                        properties: {
                            "isPlausible": { "type": "BOOLEAN" },
                            "reason": { "type": "STRING" }
                        }
                    }
                }
            };
            const apiKey = ""; // Canvas will provide this
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;

            try {
                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });
                const result = await response.json();
                const verificationResult = JSON.parse(result.candidates[0].content.parts[0].text);

                if (verificationResult.isPlausible) {
                    hideModal();
                    showPage(2); // Go to Gather Requirements Page
                } else {
                    showModal(`Verification failed: ${verificationResult.reason || 'Details could not be verified.'}`, false);
                }
            } catch (error) {
                console.error('Error during verification:', error);
                showModal('An error occurred during verification. Please try again.', false);
            }
        });

        // Gather Requirements Form submission
        requirementsForm.addEventListener('submit', async (e) => {
            e.preventDefault();
            const formData = new FormData(requirementsForm);
            userData.requirements = formData.getAll('requirements');
            userData.aiLevel = formData.getAll('ai-level');

            showModal("Analyzing your requirements to recommend the best plan...", true);

            // Simulate AI analysis and recommendation
            await new Promise(resolve => setTimeout(resolve, 3000)); // Simulate network delay

            // Call LLM to recommend a package based on requirements
            const prompt = `Based on the following credentialing requirements and desired AI level, recommend one of these packages: "Starter Credentialing", "Pro Compliance", "Enterprise Suite".
            Requirements: ${userData.requirements.join(', ')}
            AI Level: ${userData.aiLevel.join(', ')}
            Consider that "Enterprise Suite" is for comprehensive AI and unlimited profiles, "Pro Compliance" for advanced AI and automated checks, and "Starter Credentialing" for basic needs.
            Respond with the recommended package name only.`;

            const payload = {
                contents: [{ role: "user", parts: [{ text: prompt }] }]
            };
            const apiKey = ""; // Canvas will provide this
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;

            try {
                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });
                const result = await response.json();
                const recommendedPackageName = result.candidates[0].content.parts[0].text.trim();

                // Set the recommended package for display
                selectedPackage = packageDetails[recommendedPackageName] ? recommendedPackageName : "Pro Compliance"; // Default to Pro if AI fails

                // Highlight the recommended package on the UI
                document.querySelectorAll('.start-trial-btn').forEach(button => {
                    const packageDiv = button.closest('[data-package]');
                    if (packageDiv) {
                        const packageName = packageDiv.dataset.package;
                        if (packageName === selectedPackage) {
                            packageDiv.classList.add('bg-indigo-50', 'shadow-lg', 'border-indigo-600');
                            packageDiv.classList.remove('bg-gray-50', 'border-transparent');
                            // Add "Recommended by AI" tag if it's the AI's pick
                            if (!packageDiv.querySelector('.absolute')) {
                                const recommendedTag = document.createElement('span');
                                recommendedTag.className = 'absolute top-0 right-0 -mt-4 -mr-4 bg-indigo-600 text-white text-xs font-bold px-3 py-1 rounded-full shadow-md';
                                recommendedTag.textContent = 'Recommended by AI';
                                packageDiv.prepend(recommendedTag);
                            }
                        } else {
                            packageDiv.classList.remove('bg-indigo-50', 'shadow-lg', 'border-indigo-600');
                            packageDiv.classList.add('bg-gray-50', 'border-transparent');
                            const existingTag = packageDiv.querySelector('.absolute');
                            if (existingTag) existingTag.remove();
                        }
                    }
                });

                hideModal();
                showPage(3); // Go to Subscription Packages Page
            } catch (error) {
                console.error('Error during AI recommendation:', error);
                showModal('An error occurred during recommendation. Please choose a plan manually.', false);
                showPage(3); // Still go to packages page even if AI fails
            }
        });

        // Event listener for "Start 7-Day Trial" and "Sign Up Now" buttons on Subscription Packages Page
        document.querySelectorAll('.start-trial-btn, .sign-up-now-btn').forEach(button => {
            button.addEventListener('click', (e) => {
                selectedPackage = e.target.dataset.package;
                userData.selectedPackage = selectedPackage; // Store selected package in user data
                showPage(4); // Go to Collect Account Details Page
            });
        });

        // Collect Account Details Form submission
        accountDetailsForm.addEventListener('submit', async (e) => {
            e.preventDefault();
            const formData = new FormData(accountDetailsForm);
            userData.billingDetails = Object.fromEntries(formData.entries());

            showModal("Processing your billing details...", true);

            // Simulate API call for billing details processing
            await new Promise(resolve => setTimeout(resolve, 2000)); // Simulate network delay

            hideModal();
            populateEstimateAndShowPage();
        });

        // Function to populate estimate details and show the page
        function populateEstimateAndShowPage() {
            const estimatePackageElem = document.getElementById('estimate-package');
            const estimateCostElem = document.getElementById('estimate-cost');
            const estimateFeaturesElem = document.getElementById('estimate-features');
            const estimateTotalElem = document.getElementById('estimate-total');

            const packageInfo = packageDetails[selectedPackage];

            if (packageInfo) {
                estimatePackageElem.textContent = selectedPackage;
                estimateCostElem.textContent = `£${packageInfo.cost}.00`;
                estimateTotalElem.textContent = `£${packageInfo.cost}.00`; // Assuming monthly cost is total due for simplicity

                // Clear previous features
                estimateFeaturesElem.innerHTML = '';
                packageInfo.features.forEach(feature => {
                    const li = document.createElement('li');
                    li.textContent = feature;
                    estimateFeaturesElem.appendChild(li);
                });
            }

            showPage(5); // Go to Estimate & Payment Page
        }

        // Proceed to Payment button
        proceedPaymentBtn.addEventListener('click', async () => {
            showModal("Processing payment...", true);

            // Simulate payment processing
            await new Promise(resolve => setTimeout(resolve, 3000)); // Simulate network delay

            // Call LLM to generate a simple contract text
            const contractPrompt = `Generate a very brief, generic contract confirmation message for a Credentially subscription.
            Include the organization name: ${userData['org-name'] || 'your organization'},
            the selected package: ${selectedPackage || 'the chosen package'},
            and mention that a full contract has been sent to their email: ${userData.email || 'their email address'}.
            Keep it concise and professional.`;

            const payload = {
                contents: [{ role: "user", parts: [{ text: contractPrompt }] }]
            };
            const apiKey = ""; // Canvas will provide this
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;

            try {
                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });
                const result = await response.json();
                const contractMessage = result.candidates[0].content.parts[0].text.trim();

                // Update the contract setup page message with the generated text
                document.querySelector('#contract-setup-page p:nth-of-type(2)').textContent = contractMessage;

                // Show personal assistance message if applicable
                if (packageDetails[selectedPackage] && packageDetails[selectedPackage].personalAssistance) {
                    personalAssistanceMessage.classList.remove('hidden');
                } else {
                    personalAssistanceMessage.classList.add('hidden');
                }

                hideModal();
                showPage(6); // Go to Contract & Setup Page
            } catch (error) {
                console.error('Error generating contract:', error);
                showModal('Payment successful, but an error occurred generating contract details. Please contact support.', false);
                showPage(6); // Still go to contract page
            }
        });

        // Go to Dashboard button
        goToDashboardBtn.addEventListener('click', () => {
            showModal("Redirecting to your Credentially Dashboard...", true);
            // In a real app, this would redirect to the actual dashboard
            setTimeout(() => {
                hideModal();
                alert('Welcome to your Credentially Dashboard! (This is a prototype, no actual dashboard exists)');
                // Optionally reset the prototype or redirect
                showPage(0); // Go back to landing page
                infoForm.reset();
                requirementsForm.reset();
                accountDetailsForm.reset();
                userData = {};
                selectedPackage = null;
            }, 1500);
        });

        // Modal OK button and close button
        modalOkBtn.addEventListener('click', hideModal);
        modalCloseBtn.addEventListener('click', hideModal);

        // Close modal if clicked outside (optional, but good UX)
        window.addEventListener('click', (event) => {
            if (event.target === messageModal) {
                hideModal();
            }
        });

        // Initial page load
        showPage(0);
    </script>
</body>
</html>

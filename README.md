<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MIAMI Waymo Depot Report Generator</title>
    <!-- Load Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Custom styles for better aesthetics */
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f7f9fc;
        }
        .container-card {
            box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1), 0 0 10px -5px rgba(0, 0, 0, 0.04);
        }
        .input-group label {
            font-weight: 500;
            color: #374151;
        }
        .dynamic-list-container {
            border-left: 3px solid #3b82f6;
            padding-left: 1rem;
        }
    </style>
</head>
<body class="p-4 sm:p-8">

    <div id="app" class="max-w-4xl mx-auto container-card bg-white p-6 sm:p-10 rounded-xl">

        <!-- Header -->
        <header class="text-center mb-8 border-b pb-4">
            <h1 class="text-3xl sm:text-4xl font-extrabold text-blue-700 tracking-tight">MIAMI Waymo Depot Report</h1>
            <p id="timestamp" class="text-sm text-gray-500 mt-2"></p>
        </header>

        <!-- Input Form -->
        <div class="space-y-8">
            <!-- Basic Details and Headcount -->
            <section class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <!-- Shift Selection -->
                <div class="md:col-span-1 input-group">
                    <label for="shift" class="block text-sm mb-1">Shift</label>
                    <select id="shift" class="w-full p-2 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500">
                        <option value="12:00 AM - 9:00 AM">12:00 AM - 9:00 AM</option>
                        <option value="4:00 AM - 1:00 PM">4:00 AM - 1:00 PM</option>
                        <option value="8:00 AM - 5:00 PM">8:00 AM - 5:00 PM</option>
                        <option value="12:00 PM - 9:00 PM">12:00 PM - 9:00 PM</option>
                        <option value="4:00 PM - 1:00 AM">4:00 PM - 1:00 AM</option>
                        <option value="8:00 PM - 5:00 AM">8:00 PM - 5:00 AM</option>
                    </select>
                </div>

                <!-- Headcounts -->
                <div class="md:col-span-2 grid grid-cols-3 gap-4">
                    <div class="input-group">
                        <label for="requiredHC" class="block text-sm mb-1">Required HC</label>
                        <input type="number" id="requiredHC" class="w-full p-2 border border-gray-300 rounded-lg" value="0">
                    </div>
                    <div class="input-group">
                        <label for="plannedHC" class="block text-sm mb-1">Planned HC</label>
                        <input type="number" id="plannedHC" class="w-full p-2 border border-gray-300 rounded-lg" value="0">
                    </div>
                    <div class="input-group">
                        <label for="actualHC" class="block text-sm mb-1">Actual HC</label>
                        <input type="number" id="actualHC" class="w-full p-2 border border-gray-300 rounded-lg" value="0">
                    </div>
                </div>
            </section>

            <hr class="border-gray-200">

            <!-- Presence and Absence Summary -->
            <section class="grid grid-cols-1 sm:grid-cols-2 gap-4 bg-blue-50 p-4 rounded-lg">
                <div class="text-lg font-semibold text-blue-800">
                    Total Attending: <span id="totalAttending" class="text-2xl">0</span>
                </div>
                <div class="text-lg font-semibold text-red-700">
                    Total Absences: <span id="totalAbsences" class="text-2xl">0</span>
                </div>
            </section>

            <hr class="border-gray-200">

            <!-- Dynamic Personnel Lists -->
            <div class="grid grid-cols-1 md:grid-cols-2 gap-8">

                <!-- Column 1: Present Personnel -->
                <div class="space-y-6">
                    <h2 class="text-xl font-bold text-gray-700">Personnel Breakdown (Present)</h2>

                    <!-- Mission MDC -->
                    <div class="dynamic-list-container">
                        <h3 class="text-lg font-semibold text-gray-800 mb-2">Mission MDC</h3>
                        <div id="missionMDC_inputs" class="space-y-2 mb-2"></div>
                        <button onclick="addInput('missionMDC', 'text')" class="text-blue-600 hover:text-blue-800 text-sm font-medium transition duration-150 ease-in-out">+ Add Personnel</button>
                    </div>

                    <!-- Mission Mapping -->
                    <div class="dynamic-list-container">
                        <h3 class="text-lg font-semibold text-gray-800 mb-2">Mission Mapping</h3>
                        <div id="missionMapping_inputs" class="space-y-2 mb-2"></div>
                        <button onclick="addInput('missionMapping', 'text')" class="text-blue-600 hover:text-blue-800 text-sm font-medium transition duration-150 ease-in-out">+ Add Personnel</button>
                    </div>

                    <!-- Mission ADC -->
                    <div class="dynamic-list-container">
                        <h3 class="text-lg font-semibold text-gray-800 mb-2">Mission ADC</h3>
                        <div id="missionADC_inputs" class="space-y-2 mb-2"></div>
                        <button onclick="addInput('missionADC', 'text')" class="text-blue-600 hover:text-blue-800 text-sm font-medium transition duration-150 ease-in-out">+ Add Personnel</button>
                    </div>

                    <!-- Depot -->
                    <div class="dynamic-list-container">
                        <h3 class="text-lg font-semibold text-gray-800 mb-2">Depot</h3>
                        <div id="depot_inputs" class="space-y-2 mb-2"></div>
                        <button onclick="addInput('depot', 'text')" class="text-blue-600 hover:text-blue-800 text-sm font-medium transition duration-150 ease-in-out">+ Add Personnel</button>
                    </div>

                    <!-- Trainers -->
                    <div class="dynamic-list-container">
                        <h3 class="text-lg font-semibold text-gray-800 mb-2">Trainers</h3>
                        <div id="trainers_inputs" class="space-y-2 mb-2"></div>
                        <button onclick="addInput('trainers', 'text')" class="text-blue-600 hover:text-blue-800 text-sm font-medium transition duration-150 ease-in-out">+ Add Trainer</button>
                    </div>

                     <!-- Trainees -->
                    <div class="dynamic-list-container">
                        <h3 class="text-lg font-semibold text-gray-800 mb-2">Trainees</h3>
                        <div id="trainees_inputs" class="space-y-2 mb-2"></div>
                        <button onclick="addInput('trainees', 'text')" class="text-blue-600 hover:text-blue-800 text-sm font-medium transition duration-150 ease-in-out">+ Add Trainee</button>
                    </div>

                     <!-- Buffer -->
                    <div class="dynamic-list-container">
                        <h3 class="text-lg font-semibold text-gray-800 mb-2">Buffer</h3>
                        <div id="buffer_inputs" class="space-y-2 mb-2"></div>
                        <button onclick="addInput('buffer', 'text')" class="text-blue-600 hover:text-blue-800 text-sm font-medium transition duration-150 ease-in-out">+ Add Personnel</button>
                    </div>
                </div>

                <!-- Column 2: Absence/Exception Personnel -->
                <div class="space-y-6">
                    <h2 class="text-xl font-bold text-gray-700">Absences and Exceptions</h2>

                    <!-- Absent -->
                    <div class="dynamic-list-container">
                        <h3 class="text-lg font-semibold text-gray-800 mb-2">Absent</h3>
                        <div id="absent_inputs" class="space-y-2 mb-2"></div>
                        <button onclick="addInput('absent', 'text')" class="text-blue-600 hover:text-blue-800 text-sm font-medium transition duration-150 ease-in-out">+ Add Absentee</button>
                    </div>

                    <!-- No Call No Show -->
                    <div class="dynamic-list-container">
                        <h3 class="text-lg font-semibold text-gray-800 mb-2">No Call No Show</h3>
                        <div id="ncns_inputs" class="space-y-2 mb-2"></div>
                        <button onclick="addInput('ncns', 'text')" class="text-blue-600 hover:text-blue-800 text-sm font-medium transition duration-150 ease-in-out">+ Add NCNS</button>
                    </div>

                    <!-- Late w/ Time of Arrival (Text + Time) -->
                    <div class="dynamic-list-container">
                        <h3 class="text-lg font-semibold text-gray-800 mb-2">Late w/ Time of Arrival</h3>
                        <div id="late_inputs" class="space-y-2 mb-2"></div>
                        <button onclick="addInput('late', 'text-time')" class="text-blue-600 hover:text-blue-800 text-sm font-medium transition duration-150 ease-in-out">+ Add Late Arrival</button>
                    </div>

                    <!-- Early Out w/ Time of Departure (Text + Time) -->
                    <div class="dynamic-list-container">
                        <h3 class="text-lg font-semibold text-gray-800 mb-2">Early Out w/ Time of Departure</h3>
                        <div id="earlyOut_inputs" class="space-y-2 mb-2"></div>
                        <button onclick="addInput('earlyOut', 'text-time')" class="text-blue-600 hover:text-blue-800 text-sm font-medium transition duration-150 ease-in-out">+ Add Early Out</button>
                    </div>

                    <!-- NEW: Road Events w/ Ticket ID -->
                    <div class="dynamic-list-container">
                        <h3 class="text-lg font-semibold text-gray-800 mb-2">Road Events w/ Ticket ID</h3>
                        <div id="roadEvents_inputs" class="space-y-2 mb-2"></div>
                        <button onclick="addInput('roadEvents', 'text')" class="text-blue-600 hover:text-blue-800 text-sm font-medium transition duration-150 ease-in-out">+ Add Event</button>
                    </div>

                    <!-- NEW: Rescues w/ Time of Return -->
                    <div class="dynamic-list-container">
                        <h3 class="text-lg font-semibold text-gray-800 mb-2">Rescues w/ Time of Return</h3>
                        <div id="rescues_inputs" class="space-y-2 mb-2"></div>
                        <button onclick="addInput('rescues', 'text-time')" class="text-blue-600 hover:text-blue-800 text-sm font-medium transition duration-150 ease-in-out">+ Add Rescue</button>
                    </div>
                </div>
            </div>

            <hr class="border-gray-200">

            <!-- AV & Car Metrics -->
            <section class="space-y-4">
                <h2 class="text-xl font-bold text-gray-700">Vehicle Metrics</h2>

                <!-- First/Last Car Out -->
                <div class="grid grid-cols-2 gap-4">
                    <div class="input-group">
                        <label for="firstCar" class="block text-sm mb-1">First Car Out</label>
                        <input type="time" id="firstCar" class="w-full p-2 border border-gray-300 rounded-lg">
                    </div>
                    <div class="input-group">
                        <label for="lastCar" class="block text-sm mb-1">Last Car Out</label>
                        <input type="time" id="lastCar" class="w-full p-2 border border-gray-300 rounded-lg">
                    </div>
                </div>

                <!-- Total AVs -->
                <div class="grid grid-cols-4 gap-4">
                    <div class="input-group">
                        <label for="totalAVs" class="block text-sm mb-1">Total AV's</label>
                        <input type="number" id="totalAVs" class="w-full p-2 border border-gray-300 rounded-lg" value="0">
                    </div>
                    <div class="input-group">
                        <label for="downAVs" class="block text-sm mb-1">Total AV's Down</label>
                        <input type="number" id="downAVs" class="w-full p-2 border border-gray-300 rounded-lg" value="0">
                    </div>
                    <div class="input-group">
                        <label for="availableAVs" class="block text-sm mb-1">Total AV's Available</label>
                        <input type="number" id="availableAVs" class="w-full p-2 border border-gray-300 rounded-lg" value="0">
                    </div>
                    <div class="input-group">
                        <label for="trainingAVs" class="block text-sm mb-1">Total AV's in Training</label>
                        <input type="number" id="trainingAVs" class="w-full p-2 border border-gray-300 rounded-lg" value="0">
                    </div>
                </div>
            </section>

            <!-- Generate and Copy Output -->
            <div class="pt-6 border-t mt-8">
                <button onclick="generateReport()" class="w-full bg-green-500 hover:bg-green-600 text-white font-bold py-3 px-4 rounded-lg transition duration-200 shadow-md">
                    Generate and Preview Report Text
                </button>
            </div>

            <!-- Report Output Area -->
            <div id="outputSection" class="hidden mt-6 bg-gray-100 p-4 rounded-lg border border-gray-300">
                <h3 class="text-lg font-bold mb-3 text-gray-700">Report Output (Ready to Copy)</h3>
                <textarea id="reportOutput" rows="15" class="w-full p-3 border border-gray-400 rounded-lg font-mono text-sm resize-none"></textarea>
                <button onclick="copyToClipboard()" class="mt-3 w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded-lg transition duration-200">
                    Copy to Clipboard
                </button>
                <div id="copyStatus" class="mt-2 text-sm text-center text-green-700 hidden">Copied successfully!</div>
            </div>

        </div>
    </div>

    <script>
        // --- Global State and Initialization ---

        // Object to hold all dynamic list data
        const personnelData = {
            missionMDC: [],
            missionMapping: [],
            missionADC: [],
            depot: [],
            trainers: [],
            trainees: [],
            buffer: [],
            absent: [],
            ncns: [],
            late: [], // Stores objects: { name: 'Name', time: 'Time' }
            earlyOut: [], // Stores objects: { name: 'Name', time: 'Time' }
            roadEvents: [], // NEW: Stores text
            rescues: [] // NEW: Stores objects: { name: 'Name', time: 'Time' }
        };

        const MAX_ENTRIES = 10; // Capping at 10 for a cleaner UX/UI.

        // --- NEW: Function to auto-select shift ---
        function autoSelectShift() {
            const shifts = [
                { value: "12:00 AM - 9:00 AM", startHour: 0 },
                { value: "4:00 AM - 1:00 PM", startHour: 4 },
                { value: "8:00 AM - 5:00 PM", startHour: 8 },
                { value: "12:00 PM - 9:00 PM", startHour: 12 },
                { value: "4:00 PM - 1:00 AM", startHour: 16 },
                { value: "8:00 PM - 5:00 AM", startHour: 20 }
            ];

            const currentHour = new Date().getHours();
            let selectedShiftValue = shifts[0].value; // Default to first shift (12 AM)

            // Loop in reverse to find the most recent shift that has passed
            for (let i = shifts.length - 1; i >= 0; i--) {
                if (currentHour >= shifts[i].startHour) {
                    selectedShiftValue = shifts[i].value;
                    break;
                }
            }

            // If currentHour is 2AM (currentHour = 2), loop will find startHour: 0.
            // If currentHour is 10PM (currentHour = 22), loop will find startHour: 20.
            
            document.getElementById('shift').value = selectedShiftValue;
        }


        document.addEventListener('DOMContentLoaded', () => {
            // 1. Set Date/Time Stamp
            const now = new Date();
            const dateStr = now.toLocaleDateString('en-US', { year: 'numeric', month: '2-digit', day: '2-digit' });
            const timeStr = now.toLocaleTimeString('en-US', { hour: '2-digit', minute: '2-digit', hour12: true });
            document.getElementById('timestamp').textContent = `${dateStr} | ${timeStr}`;

            // 2. NEW: Auto-select shift
            autoSelectShift();

            // 3. Initial rendering of all empty lists
            Object.keys(personnelData).forEach(key => {
                renderList(key);
            });

            // 4. Attach change listener to headcounts for live summary update
            ['requiredHC', 'plannedHC', 'actualHC'].forEach(id => {
                document.getElementById(id).addEventListener('input', updateSummary);
            });
        });

        // --- Core Functions ---

        function updateSummary() {
            const actualHC = parseInt(document.getElementById('actualHC').value) || 0;

            // Count hard absences (NOT attending)
            const hardAbsenceCount = personnelData.absent.length + personnelData.ncns.length;
            
            // NEW: Count all absences AND exceptions
            const totalExceptionCount = personnelData.absent.length +
                                        personnelData.ncns.length +
                                        personnelData.late.length +
                                        personnelData.earlyOut.length +
                                        personnelData.roadEvents.length +
                                        personnelData.rescues.length;

            // Calculate Total Attending (based on Actual HC minus those *not* attending)
            // People who are late, early out, on road events, or rescues are still considered "attending".
            const totalAttending = actualHC - hardAbsenceCount;

            document.getElementById('totalAbsences').textContent = totalExceptionCount; // Use the new total count here
            document.getElementById('totalAttending').textContent = Math.max(0, totalAttending); // This logic remains the same
        }

        function removeInput(section, index) {
            personnelData[section].splice(index, 1);
            renderList(section);
            updateSummary();
        }

        function updatePersonnelData(section, index, type, value) {
            if (type === 'text') {
                personnelData[section][index] = value;
            } else if (type === 'name') {
                // For 'text-time' lists (late/earlyOut/rescues)
                if (personnelData[section][index]) {
                    personnelData[section][index].name = value;
                } else {
                    personnelData[section][index] = { name: value, time: '' };
                }
            } else if (type === 'time') {
                 // For 'text-time' lists (late/earlyOut/rescues)
                if (personnelData[section][index]) {
                    personnelData[section][index].time = value;
                } else {
                    personnelData[section][index] = { name: '', time: value };
                }
            }
        }

        function renderList(section) {
            const container = document.getElementById(section + '_inputs');
            if (!container) return;

            container.innerHTML = ''; // Clear existing inputs

            personnelData[section].forEach((item, index) => {
                // Check if this section type requires text + time
                const isTextTime = ['late', 'earlyOut', 'rescues'].includes(section);
                const isTextOnly = !isTextTime;

                const wrapper = document.createElement('div');
                wrapper.className = isTextTime ? 'flex gap-2 items-center' : 'flex gap-2 items-center';

                // --- Input Field(s) ---
                if (isTextOnly) {
                    const input = document.createElement('input');
                    input.type = 'text';
                    input.placeholder = (section === 'roadEvents') ? 'Event w/ Ticket ID' : 'Name';
                    input.value = item || '';
                    input.className = 'flex-grow p-2 border border-gray-300 rounded-lg text-sm';
                    input.oninput = (e) => {
                        updatePersonnelData(section, index, 'text', e.target.value);
                    };
                    wrapper.appendChild(input);
                } else { // text-time (Late / Early Out / Rescues)
                    const nameInput = document.createElement('input');
                    nameInput.type = 'text';
                    nameInput.placeholder = (section === 'rescues') ? 'Rescue w/ Vehicle' : 'Name';
                    nameInput.value = item.name || '';
                    nameInput.className = 'flex-grow p-2 border border-gray-300 rounded-lg text-sm';
                    nameInput.oninput = (e) => {
                        updatePersonnelData(section, index, 'name', e.target.value);
                    };

                    const timeInput = document.createElement('input');
                    timeInput.type = 'time';
                    timeInput.value = item.time || '';
                    timeInput.className = 'w-24 p-2 border border-gray-300 rounded-lg text-sm';
                    timeInput.oninput = (e) => {
                        updatePersonnelData(section, index, 'time', e.target.value);
                    };

                    wrapper.appendChild(nameInput);
                    wrapper.appendChild(timeInput);
                }

                // --- Remove Button ---
                const removeBtn = document.createElement('button');
                removeBtn.type = 'button';
                removeBtn.className = 'text-red-500 hover:text-red-700 p-1 transition duration-150';
                removeBtn.innerHTML = `
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
                        <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zM7 9a1 1 0 000 2h6a1 1 0 100-2H7z" clip-rule="evenodd" />
                    </svg>
                `;
                removeBtn.onclick = () => removeInput(section, index);
                wrapper.appendChild(removeBtn);

                container.appendChild(wrapper);
            });

            // Disable Add button if max entries reached
            const addButton = container.nextElementSibling;
            if (addButton && addButton.tagName === 'BUTTON') {
                addButton.disabled = personnelData[section].length >= MAX_ENTRIES;
                addButton.classList.toggle('opacity-50', addButton.disabled);
                addButton.classList.toggle('cursor-not-allowed', addButton.disabled);
            }
        }

        window.addInput = (section, type) => {
            if (personnelData[section].length < MAX_ENTRIES) {
                if (type === 'text-time') {
                    personnelData[section].push({ name: '', time: '' });
                } else {
                    personnelData[section].push('');
                }
                renderList(section);
                updateSummary();
            } else {
                console.warn(`Max entries (${MAX_ENTRIES}) reached for ${section}.`);
            }
        };


        // --- Report Generation ---

        window.generateReport = () => {
            const outputArea = document.getElementById('reportOutput');
            const outputSection = document.getElementById('outputSection');
            const shift = document.getElementById('shift').value;
            const timestamp = document.getElementById('timestamp').textContent;
            
            // Collect Headcounts
            const requiredHC = document.getElementById('requiredHC').value;
            const plannedHC = document.getElementById('plannedHC').value;
            const actualHC = document.getElementById('actualHC').value;

            // Collect AV Metrics
            const firstCar = document.getElementById('firstCar').value;
            const lastCar = document.getElementById('lastCar').value;
            const totalAVs = document.getElementById('totalAVs').value;
            const downAVs = document.getElementById('downAVs').value;
            const availableAVs = document.getElementById('availableAVs').value;
            const trainingAVs = document.getElementById('trainingAVs').value;

            // Get Summary Counts
            const totalAttending = document.getElementById('totalAttending').textContent;
            const totalAbsences = document.getElementById('totalAbsences').textContent;


            // Helper to format dynamic lists
            const formatList = (section, formatter = (item) => item) => {
                const list = personnelData[section].map(formatter).filter(s => (s.trim() !== '' && s.trim() !== '@'));
                return list.length > 0 ? '\n' + list.join('\n') : ' N/A';
            };
            
            // Formatters for specific lists
            const nameTimeFormatter = (item) => {
                const name = item.name || '[Entry Missing]';
                const time = item.time ? ` @ ${item.time}` : ' [Time Missing]';
                return `${name}${time}`;
            };
            
            const textFormatter = (item) => item;

            // Build the Report Text
            const reportText = `
MIAMI Waymo Depot Report
${timestamp}
Shift: ${shift}

Head Count Summary
-----------------------------------
Required Head Count: ${requiredHC}
Planned Head Count: ${plannedHC}
Actual Head Count: ${actualHC}
Total Attending (Calculated): ${totalAttending}
Total Absences (Calculated): ${totalAbsences}


Personnel Breakdown (Present)
-----------------------------------
Mission: MDC:${formatList('missionMDC', textFormatter)}

Mission: Mapping:${formatList('missionMapping', textFormatter)}

Mission: ADC:${formatList('missionADC', textFormatter)}

Depot:${formatList('depot', textFormatter)}

Trainers:${formatList('trainers', textFormatter)}

Trainees:${formatList('trainees', textFormatter)}

Buffer:${formatList('buffer', textFormatter)}


Absences and Exceptions
-----------------------------------
Absent:${formatList('absent', textFormatter)}

No Call No Show:${formatList('ncns', textFormatter)}

Late w/ Time of Arrival:${formatList('late', nameTimeFormatter)}

Early Out w/ Time of Departure:${formatList('earlyOut', nameTimeFormatter)}

Road Events w/ Ticket ID:${formatList('roadEvents', textFormatter)}

Rescues w/ Time of Return:${formatList('rescues', nameTimeFormatter)}


Vehicle Metrics
-----------------------------------
First car out: ${firstCar || 'N/A'}
Last car out: ${lastCar || 'N/A'}

Total AV's: ${totalAVs}
Total AV's down: ${downAVs}
Total AV's available: ${availableAVs}
Total AV's in training: ${trainingAVs}
`.trim(); // Removed .replace(/\n\n/g, '\n') to allow blank lines

            outputArea.value = reportText;
            outputSection.classList.remove('hidden');

            // Scroll to output
            outputSection.scrollIntoView({ behavior: 'smooth' });
        };

        // --- Utility Functions ---

        window.copyToClipboard = () => {
            const copyText = document.getElementById("reportOutput");
            copyText.select();
            copyText.setSelectionRange(0, 99999); // For mobile devices

            try {
                // Use execCommand for broader compatibility in sandboxed environments
                document.execCommand('copy');
                const status = document.getElementById('copyStatus');
                status.classList.remove('hidden');
                setTimeout(() => status.classList.add('hidden'), 2000);
            } catch (err) {
                console.error('Could not copy text: ', err);
                // Using a minimal custom message instead of alert()
                const status = document.getElementById('copyStatus');
                status.textContent = 'Copying failed. Please copy the text manually.';
                status.classList.remove('hidden');
                status.classList.remove('text-green-700');
                status.classList.add('text-red-700');
                setTimeout(() => {
                    status.classList.add('hidden');
                    status.classList.remove('text-red-700');
                    status.classList.add('text-green-700');
                    status.textContent = 'Copied successfully!';
                }, 4000);
            }
        };

        // Expose function for initial call if needed
        window.updateSummary = updateSummary;

    </script>
</body>
</html>




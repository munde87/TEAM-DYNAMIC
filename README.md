# TEAM-DYNAMIC
#define BLYNK_TEMPLATE_ID "TMPL3dZVNeIHP"
#define BLYNK_TEMPLATE_NAME "Digital Farming"
#define BLYNK_AUTH_TOKEN "LCEUyg6nH_hGCrJaS3E-aakh5pxHCkGY"

#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <DHT.h>
#include <WiFi.h>
#include <WiFiClient.h>
#include <BlynkSimpleEsp32.h>

// Replace with your WiFi credentials
char auth[] = BLYNK_AUTH_TOKEN ;
char ssid[] = "Wokwi-GUEST";
char pass[] = "";

// Blynk virtual pins
#define BLYNK_VIRTUAL_PIN V0
#define BLYNK_TEMPERATURE_PIN V1
#define BLYNK_HUMIDITY_PIN V2
// normal pins
#define POTENTIOMETER_PIN 34
#define LED1 12
#define RELAY 13
#define RELAY2 14
#define RELAY3 15

// DHT22 Configuration
#define DHT_PIN 12    // DHT22 data pin
#define DHT_TYPE DHT22
DHT dht(DHT_PIN, DHT_TYPE);

// LCD I2C Configuration
LiquidCrystal_I2C lcd(0x27, 16, 2);  // Set LCD I2C address (usually 0x27)

// Variables
int potentiometerValue;
float voltage, temperature, humidity;

// Timer to send data periodically
BlynkTimer timer;

int SW_State_M = 0;
BLYNK_WRITE(V3) {
  SW_State_M = param.asInt();
  if (SW_State_M == 1) {
    digitalWrite(LED1,LOW);
    digitalWrite(RELAY, LOW);
    Serial.println("RED ON");
    Blynk.virtualWrite(V3, LOW);
  } else {
    digitalWrite(LED1, HIGH);
    digitalWrite(RELAY, HIGH);
    Serial.println("RED OFF");
    Blynk.virtualWrite(V3, HIGH);
  }
}

int Relay2 = 0;
BLYNK_WRITE(V4) {
  Relay2 = param.asInt();
  if (Relay2 == 1) {
    digitalWrite(LED1,LOW);
    digitalWrite(RELAY2, LOW);
    Serial.println("RED ON");
    Blynk.virtualWrite(V4, LOW);
  } else {
    digitalWrite(LED1, HIGH);
    digitalWrite(RELAY2, HIGH);
    Serial.println("RED OFF");
    Blynk.virtualWrite(V4, HIGH);
  }
}

int Relay3 = 0;
BLYNK_WRITE(V5) {
  Relay3 = param.asInt();
  if (Relay3 == 1) {
    digitalWrite(LED1,LOW);
    digitalWrite(RELAY3, LOW);
    Serial.println("RED ON");
    Blynk.virtualWrite(V5, LOW);
  } else {
    digitalWrite(LED1, HIGH);
    digitalWrite(RELAY3, HIGH);
    Serial.println("RED OFF");
    Blynk.virtualWrite(V5, HIGH);
  }
}

// Function to read and send potentiometer data
void sendPotentiometerData() {
  potentiometerValue = analogRead(POTENTIOMETER_PIN);   // Read potentiometer value
  voltage = potentiometerValue * (3.3 / 4095.0);       // Convert to voltage for ESP32
  Blynk.virtualWrite(BLYNK_VIRTUAL_PIN, potentiometerValue);  // Send data to Blynk App
  Serial.print("Potentiometer Value: ");
  Serial.println(potentiometerValue);  // Print to Serial Monitor
}

// Function to read and send DHT22 data
void sendDHTData() {
  temperature = dht.readTemperature();  // Read temperature
  humidity = dht.readHumidity();        // Read humidity



  Blynk.virtualWrite(BLYNK_TEMPERATURE_PIN, temperature);  // Send temperature to Blynk
  Blynk.virtualWrite(BLYNK_HUMIDITY_PIN, humidity);        // Send humidity to Blynk

  Serial.print("Temperature: ");
  Serial.print(temperature);
  Serial.print(" °C, Humidity: ");
  Serial.print(humidity);
  Serial.println(" %");

  // Display on LCD
  lcd.setCursor(0, 0);
  lcd.print("Temp: ");
  lcd.print(temperature);
  lcd.print(" C");
  lcd.setCursor(0, 1);
  lcd.print("Hum: ");
  lcd.print(humidity);
  lcd.print(" %");
}

void setup() {
  // Start serial communication
  Serial.begin(9600);

  // Initialize pins
  pinMode(DHT_PIN, INPUT);
  
  pinMode(POTENTIOMETER_PIN, INPUT);
  pinMode(LED1, OUTPUT);
  pinMode(RELAY, OUTPUT);

  // Start DHT sensor
  dht.begin();

  // Initialize LCD
  lcd.init();
  lcd.backlight();

  // Start Blynk
  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);

  // Setup a function to be called every second
  timer.setInterval(1000L, sendPotentiometerData);
  timer.setInterval(2000L, sendDHTData);  // Call DHT22 function every 2 seconds
}

void loop() {
  Blynk.run();  // Run Blynk connection
  timer.run();  // Run timer to send data periodically
}

















<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title> Style Sign-In</title>
    <link rel="stylesheet" href="index.css">
    <script src="https://kit.fontawesome.com/a076d05399.js" crossorigin="anonymous"></script>
   
</head>
<body>
    <video class="video-bg" autoplay loop muted playsinline>
        <source src="My CHannel (1).mp4" type="video/mp4">
    </video>
    
    <div class="signin-container">
        <div class="signin-box">
            <button type="submit" class="signin-button facebook" onclick="window.location.href='mainhomepage.htm'">
                <i class="fa-brands fa-facebook"></i>
                Visit our website
            </button>
            <div class="options">
                <button type="submit" class="signin-buttonguest" onclick="window.location.href='createaccount.htm'">
                    <i class="fa-solid fa-user"></i>
                    Guest
                </button>
                <button type="submit" class="signin-buttonemail" onclick="window.location.href='emaillogin.htm'">
                    <i class="fa-solid fa-envelope"></i>
                    Email
                </button>
            </div>
            <div class="terms">
                <label><input type="checkbox"> I agree to the <a href="#">Terms of Service</a> and <a href="#">Privacy Policy</a>.</label>
                <label><input type="checkbox"> I am over the age of majority or have my guardian’s approval.</label>
            </div>
        </div>
    </div>
</body>
</html>




















home page
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Code Snippet Manager</title>
  <link rel="stylesheet" href="./style.css">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
</head>
<body>
  <header>
    <div class="header-container">
      <div class="logo-container">
        <img src="./TPAS-removebg-preview.png" alt="Logo" class="logo-img"> <!-- Add your logo image here -->
      </div>
      <div class="search-container">
        <input type="text" id="search-input" placeholder="Search Here...">
        <button id="search-button"><i class="fa-solid fa-search"></i></button>
      </div>
      <div class="user-actions">
        <div class="menu-bar">
          <div class="menu">
          
            <select class="menu" id="menu-dropdown"> 
              <option value="#">MENU</option>
              <option value="profile.html">PROFILE</option>
              <option value="chatbot.html">CHATBOT</option>
              <option value="setting.htm">SETTINGS</option>
              <option value="aboutus.html">ABOUT US</option>
              <option value="exe.html">EXECUTE</option>
              <option value="tags.html">TAGS</option>
            </select>
            
            <script>
              const menuDropdown = document.getElementById("menu-dropdown");
            
              menuDropdown.addEventListener("change", () => {
                const selectedValue = menuDropdown.value;
                if (selectedValue !== "#") { // Prevent navigation for placeholder option
                  window.location.href = selectedValue;
                }
              });
            </script>
            
        </div>
        <button id="new-snippet-btn" class="primary-btn">New Snippet</button>
      </div>
    </div>
  </header>

  <main>
    <div class="sidebar">
      <h3>Files</h3>
      <div id="files-list" class="files-list">
        <!-- Saved snippets will appear here -->
        <div class="empty-state">
          <i class="fa-regular fa-folder-open"></i>
          <p>No saved snippets yet</p>
        </div>
      </div>
    </div>
    
    <div class="content">
      <div class="editor-container">
        <div class="editor-header">
          <input type="text" id="snippet-title" placeholder="Untitled Snippet">
          <div class="editor-tabs">
            <button class="tab-btn active" data-lang="html">HTML</button>
            <button class="tab-btn" data-lang="css">CSS</button>
            <button class="tab-btn" data-lang="js">JavaScript</button>
          </div>
        </div>
        
        <div class="editor-content">
          <div class="editor-pane active" id="html-editor">
            <textarea id="html-code" placeholder="Enter HTML code here..."></textarea>
          </div>
          <div class="editor-pane" id="css-editor">
            <textarea id="css-code" placeholder="Enter CSS code here..."></textarea>
          </div>
          <div class="editor-pane" id="js-editor">
            <textarea id="js-code" placeholder="Enter JavaScript code here..."></textarea>
          </div>
        </div>
        
        <div class="preview-container">
          <div class="preview-header">
            <h3>Preview</h1>
            <button id="refresh-preview"><i class="fa-solid fa-rotate-right"></i> Refresh</button>
          </div>
          <iframe id="preview-frame"></iframe>
        </div>
      </div>
      
      <div class="action-buttons">
        <button id="save-btn" class="primary-btn"><i class="fa-solid fa-save"></i> Save Snippet</button>
        <button id="delete-btn" class="danger-btn" disabled><i class="fa-solid fa-trash"></i> Delete Snippet</button>
      </div>
    </div>
  </main>

  <script src="./java.js"></script>
</body>
</html>








tags 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Code Snippet Tag Manager</title>
    <style>
        :root {
            --primary-color: #1e90ff;
            --dark-bg: #2c2c2c;
            --light-bg: #f0f0f0;
            --text-color: #333;
            --border-radius: 8px;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background-color: #e0e0e0;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }
        
        .app-container {
            width: 600px;
            background-color: var(--light-bg);
            border-radius: 12px;
            overflow: hidden;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
        }
        
        .app-header {
            background-color: var(--dark-bg);
            color: white;
            padding: 15px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .header-controls {
            display: flex;
            gap: 10px;
        }
        
        .header-button {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            border: none;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            background-color: #444;
            color: white;
            font-size: 14px;
        }
        
        .header-button.close {
            background-color: #ff9999;
        }
        
        .app-title {
            padding: 20px;
        }
        
        .app-title h1 {
            font-size: 36px;
            color: var(--text-color);
            font-weight: 800;
        }
        
        .main-content {
            padding: 0 20px 20px;
        }
        
        .tag-input-container {
            display: flex;
            margin-bottom: 20px;
            gap: 10px;
        }
        
        .tag-input {
            flex: 1;
            padding: 12px 15px;
            border-radius: var(--border-radius);
            border: 1px solid #ccc;
            background-color: white;
            font-size: 16px;
        }
        
        .btn {
            padding: 10px 20px;
            border-radius: var(--border-radius);
            border: none;
            cursor: pointer;
            font-weight: bold;
            font-size: 16px;
            transition: all 0.2s;
        }
        
        .btn-primary {
            background-color: var(--primary-color);
            color: white;
        }
        
        .btn-primary:hover {
            background-color: #0078e7;
        }
        
        .btn-secondary {
            background-color: #e0e0e0;
            color: var(--text-color);
        }
        
        .btn-secondary:hover {
            background-color: #d0d0d0;
        }
        
        .btn-danger {
            background-color: #e74c3c;
            color: white;
        }
        
        .btn-danger:hover {
            background-color: #c0392b;
        }
        
        .tags-container {
            margin-bottom: 20px;
        }
        
        .tag-item {
            display: flex;
            align-items: center;
            padding: 12px 15px;
            background-color: white;
            border-radius: var(--border-radius);
            margin-bottom: 10px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        
        .tag-icon {
            width: 24px;
            height: 24px;
            background-color: #444;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-right: 15px;
            color: white;
        }
        
        .tag-text {
            flex: 1;
            font-size: 16px;
        }
        
        .tag-actions {
            display: flex;
            gap: 10px;
        }
        
        .tag-button {
            background: none;
            border: none;
            cursor: pointer;
            font-size: 18px;
            color: #999;
        }
        
        .tag-button:hover {
            color: var(--primary-color);
        }
        
        .tag-button.remove:hover {
            color: #e74c3c;
        }
        
        .action-buttons {
            display: flex;
            justify-content: space-between;
            margin-top: 20px;
        }
        
        .file-input {
            display: none;
        }
        
        .slider-container {
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 20px 0;
        }
        
        .slider {
            -webkit-appearance: none;
            width: 200px;
            height: 10px;
            border-radius: 5px;
            background: #d3d3d3;
            outline: none;
            margin: 0 20px;
        }
        
        .slider::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 30px;
            height: 30px;
            border-radius: 50%;
            background: var(--primary-color);
            cursor: pointer;
        }
        
        .slider-dot {
            width: 50px;
            height: 50px;
            background-color: var(--dark-bg);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 24px;
        }
        
        .button-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            margin-top: 20px;
        }
        
        .tabs {
            display: flex;
            margin-bottom: 20px;
            border-bottom: 1px solid #ddd;
        }
        
        .tab {
            padding: 10px 20px;
            cursor: pointer;
            opacity: 0.7;
        }
        
        .tab.active {
            border-bottom: 2px solid var(--primary-color);
            opacity: 1;
            font-weight: bold;
        }
        
        .tab-content {
            display: none;
        }
        
        .tab-content.active {
            display: block;
        }
        
        .file-item {
            display: flex;
            align-items: center;
            padding: 12px 15px;
            background-color: white;
            border-radius: var(--border-radius);
            margin-bottom: 10px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        
        .file-icon {
            width: 24px;
            height: 24px;
            background-color: var(--primary-color);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-right: 15px;
            color: white;
        }
        
        .file-details {
            flex: 1;
        }
        
        .file-name {
            font-weight: bold;
            margin-bottom: 3px;
        }
        
        .file-tags {
            font-size: 12px;
            color: #666;
        }
        
        .file-actions {
            display: flex;
            gap: 10px;
        }
        
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0,0,0,0.5);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }
        
        .modal.active {
            display: flex;
        }
        
        .modal-content {
            background-color: white;
            padding: 20px;
            border-radius: var(--border-radius);
            width: 80%;
            max-width: 600px;
            max-height: 80vh;
            overflow-y: auto;
        }
        
        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }
        
        .modal-title {
            font-size: 20px;
            font-weight: bold;
        }
        
        .modal-close {
            background: none;
            border: none;
            font-size: 24px;
            cursor: pointer;
        }
        
        .code-editor {
            width: 100%;
            height: 300px;
            font-family: monospace;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: var(--border-radius);
            resize: vertical;
        }
        
        .file-tag-selector {
            margin-top: 15px;
        }
        
        .tag-chips {
            display: flex;
            flex-wrap: wrap;
            gap: 5px;
            margin-top: 10px;
        }
        
        .tag-chip {
            background-color: #e0e0e0;
            padding: 5px 10px;
            border-radius: 15px;
            font-size: 12px;
            display: flex;
            align-items: center;
            gap: 5px;
        }
        
        .tag-chip.selected {
            background-color: var(--primary-color);
            color: white;
        }
        
        .tag-chip-remove {
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div class="app-container">
        <div class="app-header">
            <div class="header-dots">
                <span style="display: inline-block; width: 12px; height: 12px; background-color: #ff9999; border-radius: 50%; margin-right: 5px;"></span>
                <span style="display: inline-block; width: 12px; height: 12px; background-color: #ff9999; border-radius: 50%;"></span>
            </div>
            <div class="header-controls">
                <button class="header-button">↓</button>
                <button class="header-button">⟳</button>
                <button class="header-button">⚙</button>
                <button class="header-button close">⊗</button>
            </div>
        </div>
        
        <div class="app-title">
            <h1>Code Snippet<br>Tag Manager</h1>
        </div>
        
        <div class="main-content">
            <div class="tabs">
                <div class="tab active" data-tab="tags">Code Snippets</div>
                <div class="tab" data-tab="files">Code Snippets</div>
            </div>
            
            <div id="tagsTab" class="tab-content active">
                <div class="tag-input-container">
                    <input type="text" id="tagInput" class="tag-input" placeholder="Add Tag">
                    <button id="addTagBtn" class="btn btn-primary">Add Tag</button>
                </div>
                
                <div id="tagsContainer" class="tags-container">
                    <!-- Tags will be added here dynamically -->
                </div>
            </div>
            
            <div id="filesTab" class="tab-content">
                <div class="action-buttons" style="margin-bottom: 20px;">
                    <button id="newFileBtn" class="btn btn-primary">New Code Snippet</button>
                </div>
                
                <div id="filesContainer" class="files-container">
                    <!-- Files will be added here dynamically -->
                </div>
            </div>
            
            <div class="slider-container">
                <div class="slider-dot">O</div>
                <input type="range" min="1" max="100" value="50" class="slider" id="tagSlider">
            </div>
            
            <div class="action-buttons">
                <input type="file" id="fileInput" class="file-input">
                <button id="importBtn" class="btn btn-secondary">Import</button>
                <button id="exportBtn" class="btn btn-secondary">Export</button>
                <button id="clearBtn" class="btn btn-danger">Delete</button>
            </div>
            
            <div class="button-grid">
                <button class="btn btn-secondary">Add Tag</button>
                <button class="btn btn-primary">Add Tag</button>
                <button class="btn btn-secondary">→</button>
                <button class="btn btn-secondary">Add Tag</button>
                <button class="btn btn-secondary">Add Tag</button>
                <button class="btn btn-secondary">Save Tag →</button>
                <button class="btn btn-secondary">Add Tag</button>
                <button class="btn btn-secondary">Add Tag</button>
                <button class="btn btn-secondary">Delete →</button>
            </div>
        </div>
    </div>
    
    <!-- File Editor Modal -->
    <div id="fileModal" class="modal">
        <div class="modal-content">
            <div class="modal-header">
                <div class="modal-title">Edit Code Snippet</div>
                <button class="modal-close">&times;</button>
            </div>
            <input type="text" id="fileNameInput" class="tag-input" placeholder="File name" style="margin-bottom: 10px;">
            <textarea id="codeEditor" class="code-editor" placeholder="Enter your code here..."></textarea>
            
            <div class="file-tag-selector">
                <div style="margin-bottom: 5px;">Tags:</div>
                <div id="tagChips" class="tag-chips">
                    <!-- Tag chips will be added here dynamically -->
                </div>
            </div>
            
            <div class="action-buttons" style="margin-top: 20px;">
                <button id="saveFileBtn" class="btn btn-primary">Save</button>
                <button id="cancelFileBtn" class="btn btn-secondary">Cancel</button>
            </div>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // DOM Elements
            const tagInput = document.getElementById('tagInput');
            const addTagBtn = document.getElementById('addTagBtn');
            const tagsContainer = document.getElementById('tagsContainer');
            const filesContainer = document.getElementById('filesContainer');
            const importBtn = document.getElementById('importBtn');
            const exportBtn = document.getElementById('exportBtn');
            const clearBtn = document.getElementById('clearBtn');
            const fileInput = document.getElementById('fileInput');
            const tabs = document.querySelectorAll('.tab');
            const tabContents = document.querySelectorAll('.tab-content');
            const newFileBtn = document.getElementById('newFileBtn');
            const fileModal = document.getElementById('fileModal');
            const fileNameInput = document.getElementById('fileNameInput');
            const codeEditor = document.getElementById('codeEditor');
            const tagChips = document.getElementById('tagChips');
            const saveFileBtn = document.getElementById('saveFileBtn');
            const cancelFileBtn = document.getElementById('cancelFileBtn');
            const modalClose = document.querySelector('.modal-close');
            
            // Data
            let tags = JSON.parse(localStorage.getItem('codeTags')) || [];
            let files = JSON.parse(localStorage.getItem('codeFiles')) || [];
            let currentFileId = null;
            
            // Initialize
            renderTags();
            renderFiles();
            
            // Tab switching
            tabs.forEach(tab => {
                tab.addEventListener('click', function() {
                    const tabId = this.getAttribute('data-tab');
                    
                    // Update active tab
                    tabs.forEach(t => t.classList.remove('active'));
                    this.classList.add('active');
                    
                    // Update active content
                    tabContents.forEach(content => content.classList.remove('active'));
                    document.getElementById(tabId + 'Tab').classList.add('active');
                });
            });
            
            // Event Listeners
            addTagBtn.addEventListener('click', addTag);
            tagInput.addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    addTag();
                }
            });
            
            importBtn.addEventListener('click', function() {
                fileInput.click();
            });
            
            fileInput.addEventListener('change', importData);
            exportBtn.addEventListener('click', exportData);
            clearBtn.addEventListener('click', clearData);
            
            newFileBtn.addEventListener('click', function() {
                openFileModal();
            });
            
            saveFileBtn.addEventListener('click', saveFile);
            cancelFileBtn.addEventListener('click', closeFileModal);
            modalClose.addEventListener('click', closeFileModal);
            
            // Functions
            function addTag() {
                const tagText = tagInput.value.trim();
                if (tagText) {
                    // Add tag to array if it doesn't exist
                    if (!tags.some(tag => tag.text.toLowerCase() === tagText.toLowerCase())) {
                        tags.push({
                            id: Date.now(),
                            text: tagText
                        });
                        
                        // Save to localStorage
                        saveTags();
                        
                        // Render tags
                        renderTags();
                    }
                    
                    // Clear input
                    tagInput.value = '';
                }
            }
            
            function renderTags() {
                tagsContainer.innerHTML = '';
                
                tags.forEach(tag => {
                    const tagElement = document.createElement('div');
                    tagElement.className = 'tag-item';
                    tagElement.innerHTML = `
                        <div class="tag-icon">●</div>
                        <div class="tag-text">${tag.text}</div>
                        <div class="tag-actions">
                            <button class="tag-button remove" data-id="${tag.id}" title="Remove tag">—</button>
                        </div>
                    `;
                    
                    tagsContainer.appendChild(tagElement);
                });
                
                // Add event listeners to tag buttons
                document.querySelectorAll('.tag-button.remove').forEach(button => {
                    button.addEventListener('click', function() {
                        const tagId = parseInt(this.getAttribute('data-id'));
                        removeTag(tagId);
                    });
                });
            }
            
            function renderFiles() {
                filesContainer.innerHTML = '';
                
                files.forEach(file => {
                    const fileElement = document.createElement('div');
                    fileElement.className = 'file-item';
                    
                    // Get tag names for this file
                    const fileTags = file.tagIds.map(tagId => {
                        const tag = tags.find(t => t.id === tagId);
                        return tag ? tag.text : '';
                    }).filter(Boolean).join(', ');
                    
                    fileElement.innerHTML = `
                        <div class="file-icon">📄</div>
                        <div class="file-details">
                            <div class="file-name">${file.name}</div>
                            <div class="file-tags">Tags: ${fileTags || 'None'}</div>
                        </div>
                        <div class="file-actions">
                            <button class="tag-button edit" data-id="${file.id}" title="Edit file">✎</button>
                            <button class="tag-button remove" data-id="${file.id}" title="Delete file">🗑</button>
                        </div>
                    `;
                    
                    filesContainer.appendChild(fileElement);
                });
                
                // Add event listeners to file buttons
                document.querySelectorAll('.file-item .tag-button.edit').forEach(button => {
                    button.addEventListener('click', function() {
                        const fileId = parseInt(this.getAttribute('data-id'));
                        editFile(fileId);
                    });
                });
                
                document.querySelectorAll('.file-item .tag-button.remove').forEach(button => {
                    button.addEventListener('click', function() {
                        const fileId = parseInt(this.getAttribute('data-id'));
                        removeFile(fileId);
                    });
                });
            }
            
            function removeTag(id) {
                // Remove tag
                tags = tags.filter(tag => tag.id !== id);
                
                // Update files that use this tag
                files = files.map(file => {
                    file.tagIds = file.tagIds.filter(tagId => tagId !== id);
                    return file;
                });
                
                // Save changes
                saveTags();
                saveFiles();
                
                // Update UI
                renderTags();
                renderFiles();
            }
            
            function removeFile(id) {
                if (confirm('Are you sure you want to delete this file?')) {
                    files = files.filter(file => file.id !== id);
                    saveFiles();
                    renderFiles();
                }
            }
            
            function openFileModal(fileId = null) {
                // Reset form
                fileNameInput.value = '';
                codeEditor.value = '';
                currentFileId = null;
                
                // If editing existing file
                if (fileId) {
                    const file = files.find(f => f.id === fileId);
                    if (file) {
                        fileNameInput.value = file.name;
                        codeEditor.value = file.content;
                        currentFileId = file.id;
                    }
                }
                
                // Render tag chips
                renderTagChips(fileId ? files.find(f => f.id === fileId).tagIds : []);
                
                // Show modal
                fileModal.classList.add('active');
            }
            
            function closeFileModal() {
                fileModal.classList.remove('active');
            }
            
            function renderTagChips(selectedTagIds = []) {
                tagChips.innerHTML = '';
                
                tags.forEach(tag => {
                    const isSelected = selectedTagIds.includes(tag.id);
                    const chipElement = document.createElement('div');
                    chipElement.className = tag-chip ${isSelected ? 'selected' : ''};
                    chipElement.setAttribute('data-id', tag.id);
                    chipElement.innerHTML = ${tag.text};
                    
                    chipElement.addEventListener('click', function() {
                        this.classList.toggle('selected');
                    });
                    
                    tagChips.appendChild(chipElement);
                });
            }
            
            function saveFile() {
                const fileName = fileNameInput.value.trim();
                const fileContent = codeEditor.value;
                
                if (!fileName) {
                    alert('Please enter a file name');
                    return;
                }
                
                // Get selected tag IDs
                const selectedTagIds = Array.from(document.querySelectorAll('.tag-chip.selected'))
                    .map(chip => parseInt(chip.getAttribute('data-id')));
                
                // Create or update file
                if (currentFileId) {
                    // Update existing file
                    const fileIndex = files.findIndex(f => f.id === currentFileId);
                    if (fileIndex !== -1) {
                        files[fileIndex] = {
                            ...files[fileIndex],
                            name: fileName,
                            content: fileContent,
                            tagIds: selectedTagIds,
                            updatedAt: Date.now()
                        };
                    }
                } else {
                    // Create new file
                    files.push({
                        id: Date.now(),
                        name: fileName,
                        content: fileContent,
                        tagIds: selectedTagIds,
                        createdAt: Date.now(),
                        updatedAt: Date.now()
                    });
                }
                
                // Save and update UI
                saveFiles();
                renderFiles();
                closeFileModal();
                
                // Switch to files tab
                tabs[1].click();
            }
            
            function editFile(fileId) {
                openFileModal(fileId);
            }
            
            function saveTags() {
                localStorage.setItem('codeTags', JSON.stringify(tags));
            }
            
            function saveFiles() {
                localStorage.setItem('codeFiles', JSON.stringify(files));
            }
            
            function importData(event) {
                const file = event.target.files[0];
                if (file) {
                    const reader = new FileReader();
                    reader.onload = function(e) {
                        try {
                            const data = JSON.parse(e.target.result);
                            
                            if (data.tags && Array.isArray(data.tags)) {
                                tags = data.tags;
                                saveTags();
                                renderTags();
                            }
                            
                            if (data.files && Array.isArray(data.files)) {
                                files = data.files;
                                saveFiles();
                                renderFiles();
                            }
                            
                            alert('Data imported successfully!');
                        } catch (error) {
                            alert('Error importing data: ' + error.message);
                        }
                    };
                    reader.readAsText(file);
                }
            }
            
            function exportData() {
                const data = {
                    tags: tags,
                    files: files
                };
                
                const dataStr = JSON.stringify(data, null, 2);
                const dataUri = 'data:application/json;charset=utf-8,'+ encodeURIComponent(dataStr);
                
                const exportFileName = 'code-snippets-backup.json';
                
                const linkElement = document.createElement('a');
                linkElement.setAttribute('href', dataUri);
                linkElement.setAttribute('download', exportFileName);
                linkElement.click();
            }
            
            function clearData() {
                if (confirm('Are you sure you want to delete all tags and files?')) {
                    tags = [];
                    files = [];
                    saveTags();
                    saveFiles();
                    renderTags();
                    renderFiles();
                }
            }
        });
    </script>
</body>
</html>

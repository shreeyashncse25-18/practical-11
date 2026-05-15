<html>
<head>
<title>Practical 11 - HTML5 APIs | CS25044</title>
<style>
body { margin: 0; font-family: Arial, sans-serif; background: #f0f0f0; }
h1 { text-align: center; background: black; color: white; padding: 15px; margin: 0; }
.container { display: flex; flex-direction: column; gap: 20px; padding: 20px; }
.section { background: white; border-radius: 10px; padding: 20px; box-shadow: 0 2px 8px rgba(0,0,0,0.1); }
.section h2 { margin: 0 0 15px 0; font-size: 18px; color: #333; border-bottom: 2px solid black; padding-bottom: 8px; }
button { padding: 10px 20px; background: black; color: white; border: none; border-radius: 8px; cursor: pointer; font-size: 14px; }
button:hover { background: #333; }
#location-result { margin-top: 10px; font-size: 14px; color: #444; }
.drag-container { display: flex; gap: 20px; flex-wrap: wrap; }
.drag-box { width: 380px; min-height: 200px; border: 2px dashed #aaa; border-radius: 8px; padding: 10px; background: #f9f9f9; text-align: center; font-size: 13px; color: #888; }
.drag-box.over { border-color: black; background: #fffde7; }
.drag-item { display: flex; align-items: center; gap: 10px; padding: 8px; background: #fff; border: 1px solid #ddd; border-radius: 8px; margin-bottom: 10px; cursor: grab; }
.drag-item img { width: 120px; height: 75px; object-fit: cover; border-radius: 6px; }
.drag-item span { font-weight: bold; font-size: 14px; color: #222; }
.storage-row { display: flex; gap: 10px; margin-bottom: 10px; flex-wrap: wrap; }
.storage-row input { padding: 8px; border: 1px solid #ccc; border-radius: 8px; font-size: 14px; flex: 1; }
#storage-result { margin-top: 10px; font-size: 14px; color: #444; }
</style>
</head>
<body>

<h1>Practical 11 - HTML5 APIs | CS25044</h1>

<div class="container">

    <div class="section">
        <h2>1. Geolocation API - Nagpur, Maharashtra</h2>
        <button onclick="getLocation()">Get My Location</button>
        <div id="location-result">Click the button to fetch your location.</div>
    </div>

    <div class="section">
        <h2>2. Drag and Drop API</h2>
        <div class="drag-container">
            <div class="drag-box" id="source-box">
                <p>Drag from here</p>
                <div class="drag-item" draggable="true" id="item1" ondragstart="dragStart(event)">
                    <img src="https://commons.wikimedia.org/wiki/Special:FilePath/2021_BMW_S1000R.jpg" alt="BMW S1000R">
                    <span>BMW S1000R</span>
                </div>
                <div class="drag-item" draggable="true" id="item2" ondragstart="dragStart(event)">
                    <img src="https://commons.wikimedia.org/wiki/Special:FilePath/Kawasaki_H2R_at_Isle_of_Man.jpg" alt="Kawasaki Ninja H2R">
                    <span>Kawasaki Ninja H2R</span>
                </div>
                <div class="drag-item" draggable="true" id="item3" ondragstart="dragStart(event)">
                    <img src="https://commons.wikimedia.org/wiki/Special:FilePath/Royal_Enfield_Guerrilla_450.jpg" alt="Royal Enfield Guerrilla 450">
                    <span>Royal Enfield Guerrilla 450</span>
                </div>
            </div>
            <div class="drag-box" id="drop-box" ondragover="dragOver(event)" ondragleave="dragLeave(event)" ondrop="drop(event)">
                <p>Drop here</p>
            </div>
        </div>
    </div>

    <div class="section">
        <h2>3. Web Storage API</h2>
        <div class="storage-row">
            <input type="text" id="storage-key" placeholder="Key">
            <input type="text" id="storage-value" placeholder="Value">
            <button onclick="saveData()">Save to LocalStorage</button>
        </div>
        <div class="storage-row">
            <input type="text" id="get-key" placeholder="Enter key to retrieve">
            <button onclick="getData()">Get from LocalStorage</button>
            <button onclick="clearData()">Clear All</button>
        </div>
        <div id="storage-result">No data retrieved yet.</div>
    </div>

</div>

<script>
function getLocation() {
    if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(function(position) {
            document.getElementById("location-result").innerHTML =
                "Latitude: " + position.coords.latitude + "<br>" +
                "Longitude: " + position.coords.longitude + "<br>" +
                "Location: Nagpur, Maharashtra, India";
        }, function() {
            document.getElementById("location-result").innerHTML =
                "Latitude: 21.150273<br>Longitude: 79.1042573<br>Location: Nagpur, Maharashtra, India";
        });
    } else {
        document.getElementById("location-result").innerText = "Geolocation not supported.";
    }
}

var draggedItem = null;
function dragStart(event) { draggedItem = event.target.closest(".drag-item"); }
function dragOver(event) { event.preventDefault(); event.target.closest(".drag-box").classList.add("over"); }
function dragLeave(event) { event.target.closest(".drag-box").classList.remove("over"); }
function drop(event) {
    event.preventDefault();
    var box = event.target.closest(".drag-box");
    box.classList.remove("over");
    if (draggedItem) { box.appendChild(draggedItem); draggedItem = null; }
}

function saveData() {
    var key = document.getElementById("storage-key").value;
    var value = document.getElementById("storage-value").value;
    if (key && value) { localStorage.setItem(key, value); document.getElementById("storage-result").innerText = "Saved: " + key + " = " + value; }
    else { document.getElementById("storage-result").innerText = "Please enter both key and value."; }
}
function getData() {
    var key = document.getElementById("get-key").value;
    var value = localStorage.getItem(key);
    document.getElementById("storage-result").innerText = value ? "Retrieved: " + key + " = " + value : "No data found for key: " + key;
}
function clearData() { localStorage.clear(); document.getElementById("storage-result").innerText = "All localStorage data cleared."; }
</script>

</body>
</html>

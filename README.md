# pollution-reporting-app
A web-based pollution reporting application with live location detection, photo upload, and delete functionality. Built using HTML, CSS, and JavaScript.


<!DOCTYPE html>
<html>
<head>
  <title>Pollution Reporting App</title>
  <style>
    body {
      font-family: Arial;
      background: linear-gradient(to right, #11998e, #38ef7d);
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
    }

    .container {
      background: white;
      padding: 20px;
      width: 420px;
      border-radius: 12px;
      box-shadow: 0 10px 20px rgba(0,0,0,0.2);
    }

    h2 { text-align: center; }

    input, select, textarea {
      width: 100%;
      margin: 8px 0;
      padding: 8px;
      border-radius: 6px;
      border: 1px solid #ccc;
    }

    button {
      width: 100%;
      padding: 10px;
      background: #11998e;
      color: white;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      margin-top: 5px;
    }

    button:hover { background: #0e7c74; }

    .report {
      background: #f2f2f2;
      padding: 10px;
      margin-top: 10px;
      border-radius: 6px;
      position: relative;
    }

    .report img {
      width: 100%;
      margin-top: 5px;
      border-radius: 6px;
    }

    .delete-btn {
      background: red;
      margin-top: 8px;
    }
  </style>
</head>
<body>

<div class="container">
  <h2>🌍 Pollution Reporting</h2>

  <select id="type">
    <option value="">Select Pollution Type</option>
    <option>Air Pollution</option>
    <option>Water Pollution</option>
    <option>Garbage Dump</option>
    <option>Noise Pollution</option>
  </select>

  <input type="text" id="location" placeholder="Location (auto detect available)">
  <button onclick="getLocation()">📍 Use Live Location</button>

  <textarea id="description" placeholder="Describe the issue"></textarea>

  <input type="file" id="photo" accept="image/*">

  <button onclick="submitReport()">Submit Report</button>

  <div id="reportList"></div>
</div>

<script>

function getLocation() {
  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(function(position) {
      document.getElementById("location").value =
        "Lat: " + position.coords.latitude +
        ", Long: " + position.coords.longitude;
    });
  } else {
    alert("Geolocation not supported.");
  }
}

function submitReport() {
  let type = document.getElementById("type").value;
  let location = document.getElementById("location").value;
  let description = document.getElementById("description").value;
  let photoInput = document.getElementById("photo");

  if(type === "" || location === "" || description === "") {
    alert("Please fill all fields!");
    return;
  }

  let reportDiv = document.createElement("div");
  reportDiv.classList.add("report");

  reportDiv.innerHTML = `
    <strong>Type:</strong> ${type}<br>
    <strong>Location:</strong> ${location}<br>
    <strong>Description:</strong> ${description}
  `;

  // Photo preview
  if (photoInput.files && photoInput.files[0]) {
    let reader = new FileReader();
    reader.onload = function(e) {
      let img = document.createElement("img");
      img.src = e.target.result;
      reportDiv.appendChild(img);
    };
    reader.readAsDataURL(photoInput.files[0]);
  }

  // Delete Button
  let deleteBtn = document.createElement("button");
  deleteBtn.textContent = "Delete Report";
  deleteBtn.classList.add("delete-btn");

  deleteBtn.onclick = function() {
    reportDiv.remove();
  };

  reportDiv.appendChild(deleteBtn);
  document.getElementById("reportList").appendChild(reportDiv);

  // Reset fields
  document.getElementById("type").value = "";
  document.getElementById("location").value = "";
  document.getElementById("description").value = "";
  document.getElementById("photo").value = "";
}

</script>

</body>
</html>

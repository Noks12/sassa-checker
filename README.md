document.getElementById("grantType").addEventListener("change", function () {
  const isSRD = this.value === "srd";
  document.getElementById("employmentField").style.display = isSRD ? "block" : "none";
});

document.getElementById("eligibilityForm").addEventListener("submit", function (e) {
  e.preventDefault();

  const age = parseInt(document.getElementById("age").value);
  const grantType = document.getElementById("grantType").value;
  const married = document.getElementById("married").value;
  const unemployed = document.getElementById("unemployed").value;
  const incomeRaw = document.getElementById("income").value.replace(/[^\d]/g, "");
  const income = parseInt(incomeRaw);

  let reasons = [];

  if (grantType === "oldAge") {
    if (age < 60) {
      reasons.push("❌ You must be at least 60 years old.");
    }
    if (married === "single" && income > 8070) {
      reasons.push("❌ As a single person, your income must not exceed R8,070.");
    }
    if (married === "married" && income > 16140) {
      reasons.push("❌ As a married couple, combined income must not exceed R16,140.");
    }
  }

  if (grantType === "srd") {
    if (age < 18) {
      reasons.push("❌ You must be 18 or older.");
    }
    if (unemployed !== "yes") {
      reasons.push("❌ You must be unemployed to qualify.");
    }
    if (income > 624) {
      reasons.push("❌ Your income must not exceed R624.");
    }
  }

  const resultDiv = document.getElementById("result");

  if (reasons.length === 0) {
    resultDiv.innerHTML = "✅ Based on your input, you may be eligible.";
  } else {
    resultDiv.innerHTML = `<strong>You may not qualify for the grant due to:</strong><ul>${reasons.map(r => `<li>${r}</li>`).join('')}</ul>`;
  }
});

document.getElementById("language").addEventListener("change", function () {
  const lang = this.value;

  const translations = {
    en: {
      grantLabel: "Grant Type:",
      ageLabel: "Age:",
      maritalLabel: "Marital Status:",
      incomeLabel: "Monthly Income:",
      unemployedLabel: "Unemployed?"
    },
    zu: {
      grantLabel: "Uhlobo Lomxhaso:",
      ageLabel: "Iminyaka:",
      maritalLabel: "Isimo Somshado:",
      incomeLabel: "Inzuzo Yanyanga Zonke:",
      unemployedLabel: "Awusebenzi?"
    },
    xh: {
      grantLabel: "Uhlobo Lwenkxaso-mali:",
      ageLabel: "Ubudala:",
      maritalLabel: "Ubume Botshatileyo:",
      incomeLabel: "Ingeniso Yenyanga:",
      unemployedLabel: "Awusebenzi?"
    }
  };

  const t = translations[lang];

  document.getElementById("grantLabel").textContent = t.grantLabel;
  document.getElementById("ageLabel").textContent = t.ageLabel;
  document.getElementById("maritalLabel").textContent = t.maritalLabel;
  document.getElementById("incomeLabel").textContent = t.incomeLabel;
  document.getElementById("unemployedLabel").textContent = t.unemployedLabel;
});

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>SASSA Grant Eligibility Checker</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="container">
    <h1>SASSA Grant Checker</h1>

    <label for="language">Language / Ulwimi / Ulimi:</label>
    <select id="language">
      <option value="en">English</option>
      <option value="zu">isiZulu</option>
      <option value="xh">isiXhosa</option>
    </select>

    <form id="eligibilityForm">
      <label><span id="grantLabel">Grant Type:</span>
        <select id="grantType" required>
          <option value="">-- Select --</option>
          <option value="oldAge">Old Age Grant</option>
          <option value="srd">Social Relief of Distress (SRD)</option>
        </select>
      </label>

      <label><span id="ageLabel">Age:</span>
        <input type="number" id="age" min="0" required>
      </label>

      <label><span id="maritalLabel">Marital Status:</span>
        <select id="married" required>
          <option value="">-- Select --</option>
          <option value="single">Single</option>
          <option value="married">Married</option>
        </select>
      </label>

      <label><span id="incomeLabel">Monthly Income:</span>
        <div class="currency-input">R <input type="text" id="income" required placeholder="e.g. 8,000"></div>
      </label>

      <label id="employmentField" style="display:none;"><span id="unemployedLabel">Unemployed?</span>
        <select id="unemployed">
          <option value="yes">Yes</option>
          <option value="no">No</option>
        </select>
      </label>

      <button type="submit">Check Eligibility</button>
    </form>

    <div id="result" class="result"></div>
    <p class="sassa-link">
  For official information and to check your SASSA grant status, visit the
  <a href="https://srd.sassa.gov.za/" target="_blank" rel="noopener noreferrer">official SASSA website</a>.
</p>
  </div>
  <script src="script.js"></script>

  <p><a href="language-guide.html">Need help understanding terms? Click here</a></p>
</body>
</html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Language Help - SASSA Grant Terms</title>
  <link rel="stylesheet" href="Steez.css">
</head>
<body>
  <div class="container">
    <h1>Language Help / Usizo Lolimi / Uncedo Lolwimi</h1>

    <p><a href="index.html">← Back to Grant Checker</a></p>

    <h2>Key SASSA Terms</h2>
    <table>
      <thead>
        <tr>
          <th>Term</th>
          <th>English</th>
          <th>isiZulu</th>
          <th>isiXhosa</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>Grant</td>
          <td>Grant</td>
          <td>Umxhaso</td>
          <td>Inkxaso-mali</td>
        </tr>
        <tr>
          <td>Old Age Grant</td>
          <td>Old Age Grant</td>
          <td>Umxhaso Wokuguga</td>
          <td>Inkxaso-mali Yabantu Abadala</td>
        </tr>
        <tr>
          <td>Monthly Income</td>
          <td>Monthly Income</td>
          <td>Inzuzo Yanyanga Zonke</td>
          <td>Ingeniso Yenyanga</td>
        </tr>
        <tr>
          <td>Married</td>
          <td>Married</td>
          <td>Ushadile</td>
          <td>Utshatile</td>
        </tr>
        <tr>
          <td>Unemployed</td>
          <td>Unemployed</td>
          <td>Awusebenzi</td>
          <td>Awusebenzi</td>
        </tr>
        <tr>
          <td>Eligible</td>
          <td>Eligible</td>
          <td>Ufanele Ukuthola</td>
          <td>Ufanelekile</td>
        </tr>
      </tbody>
    </table>

    <h2>How to Switch Languages</h2>
    <ul>
      <li>Go to the main page</li>
      <li>Use the dropdown labeled <strong>"Language / Ulwimi / Ulimi"</strong></li>
      <li>Select <strong>isiZulu</strong> or <strong>isiXhosa</strong> to translate the form</li>
    </ul>

    <h2>Need More Help?</h2>
    <p>Visit the official SASSA page: <a href="https://www.sassa.gov.za" target="_blank">www.sassa.gov.za</a></p>
  </div>
</body>
</html>
document.getElementById("grantType").addEventListener("change", function () {
  const isSRD = this.value === "srd";
  document.getElementById("employmentField").style.display = isSRD ? "block" : "none";
});

document.getElementById("eligibilityForm").addEventListener("submit", function (e) {
  e.preventDefault();

  const age = parseInt(document.getElementById("age").value);
  const grantType = document.getElementById("grantType").value;
  const married = document.getElementById("married").value;
  const unemployed = document.getElementById("unemployed").value;
  const incomeRaw = document.getElementById("income").value.replace(/[^\d]/g, "");
  const income = parseInt(incomeRaw);

  let reasons = [];

  if (grantType === "oldAge") {
    if (age < 60) {
      reasons.push("❌ You must be at least 60 years old.");
    }
    if (married === "single" && income > 8070) {
      reasons.push("❌ As a single person, your income must not exceed R8,070.");
    }
    if (married === "married" && income > 16140) {
      reasons.push("❌ As a married couple, combined income must not exceed R16,140.");
    }
  }

  if (grantType === "srd") {
    if (age < 18) {
      reasons.push("❌ You must be 18 or older.");
    }
    if (unemployed !== "yes") {
      reasons.push("❌ You must be unemployed to qualify.");
    }
    if (income > 624) {
      reasons.push("❌ Your income must not exceed R624.");
    }
  }

  const resultDiv = document.getElementById("result");

  if (reasons.length === 0) {
    resultDiv.innerHTML = "✅ Based on your input, you may be eligible.";
  } else {
    resultDiv.innerHTML = `<strong>You may not qualify for the grant due to:</strong><ul>${reasons.map(r => `<li>${r}</li>`).join('')}</ul>`;
  }
});

document.getElementById("language").addEventListener("change", function () {
  const lang = this.value;

  const translations = {
    en: {
      grantLabel: "Grant Type:",
      ageLabel: "Age:",
      maritalLabel: "Marital Status:",
      incomeLabel: "Monthly Income:",
      unemployedLabel: "Unemployed?"
    },
    zu: {
      grantLabel: "Uhlobo Lomxhaso:",
      ageLabel: "Iminyaka:",
      maritalLabel: "Isimo Somshado:",
      incomeLabel: "Inzuzo Yanyanga Zonke:",
      unemployedLabel: "Awusebenzi?"
    },
    xh: {
      grantLabel: "Uhlobo Lwenkxaso-mali:",
      ageLabel: "Ubudala:",
      maritalLabel: "Ubume Botshatileyo:",
      incomeLabel: "Ingeniso Yenyanga:",
      unemployedLabel: "Awusebenzi?"
    }
  };

  const t = translations[lang];

  document.getElementById("grantLabel").textContent = t.grantLabel;
  document.getElementById("ageLabel").textContent = t.ageLabel;
  document.getElementById("maritalLabel").textContent = t.maritalLabel;
  document.getElementById("incomeLabel").textContent = t.incomeLabel;
  document.getElementById("unemployedLabel").textContent = t.unemployedLabel;
});

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: #f5f7fa;
  color: #333;
  padding: 20px;
  line-height: 1.6;
}

/* Container */
.container {
  max-width: 700px;
  margin: auto;
  background: #ffffff;
  padding: 30px;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

/* Headings */
h1, h2 {
  color: #003366;
  margin-bottom: 20px;
}

h1 {
  font-size: 1.8rem;
  border-bottom: 2px solid #007bff;
  padding-bottom: 10px;
}

h2 {
  font-size: 1.4rem;
}

/* Labels & Forms */
label {
  display: block;
  margin-bottom: 15px;
  font-weight: bold;
}

input,
select {
  width: 100%;
  padding: 10px;
  margin-top: 5px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 1rem;
}

button {
  background-color: #007bff;
  color: white;
  padding: 12px 18px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 1rem;
  margin-top: 10px;
}

button:hover {
  background-color: #0056b3;
}

/* Currency Input */
.currency-input {
  display: flex;
  align-items: center;
}

.currency-input input {
  margin-left: 5px;
}

/* Result Box */
.result {
  margin-top: 20px;
  background-color: #e6f0ff;
  border-left: 6px solid #007bff;
  padding: 15px;
  font-weight: bold;
  border-radius: 5px;
}

/* Table Styles */
table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

table th,
table td {
  border: 1px solid #ccc;
  padding: 10px;
  text-align: left;
}

table th {
  background-color: #f0f4ff;
  color: #003366;
}

/* Links */
a {
  color: #007bff;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}

/* Responsive */
@media (max-width: 600px) {
  .container {
    padding: 20px;
  }

  input, select, button {
    font-size: 0.95rem;
  }
}
body {
  font-family: Arial, sans-serif;
  background-color: #f9f9f9;
  padding: 20px;
}

.container {
  max-width: 600px;
  margin: auto;
  background: #fff;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

label {
  display: block;
  margin-bottom: 15px;
}

input, select {
  padding: 8px;
  width: 100%;
  margin-top: 5px;
}

button {
  padding: 10px 15px;
  background-color: #007bff;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 5px;
}

.result {
  margin-top: 20px;
  padding: 10px;
  background-color: #eef;
  border-left: 5px solid #00f;
}

.currency-input {
  display: flex;
  align-items: center;
}
.sassa-link {
  margin-top: 15px;
  font-size: 0.9em;
  color: #555;
  text-align: center;
}
.sassa-link a {
  color: #007bff;
  text-decoration: none;
}
.sassa-link a:hover {
  text-decoration: underline;
}

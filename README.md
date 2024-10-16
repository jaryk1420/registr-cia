<!DOCTYPE html>
<html lang="sk">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title> STOJKA NA RUKÁCH - začiatočníci</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f7f7f7;
            margin: 0;
            padding: 0;
            color: #1b3b5f;
        }
        .container {
            max-width: 500px;
            margin: 50px auto;
            background-color: #fff9db;
            padding: 20px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            border-radius: 8px;
            text-align: center;
        }
        h1 {
            color: #1b3b5f;
        }
        p {
            font-size: 16px;
            line-height: 1.5;
            margin: 10px 0;
        }
        .price {
            font-weight: bold;
            color: #d9534f;
        }
        form {
            display: flex;
            flex-direction: column;
            align-items: flex-start;
        }
        label {
            margin-top: 10px;
        }
        input[type="text"], input[type="email"], input[type="tel"], input[type="number"] {
            width: 100%;
            padding: 10px;
            margin-top: 5px;
            border: 1px solid #ccc;
            border-radius: 4px;
        }
        input[type="checkbox"] {
            margin-right: 10px;
        }
        button {
            margin-top: 20px;
            padding: 10px;
            background-color: #28a745;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            width: 100%;
        }
        button:hover {
            background-color: #218838;
        }
        .notification {
            display: none;
            margin-top: 20px;
            padding: 15px;
            background-color: #dff0d8;
            color: #3c763d;
            border: 1px solid #d6e9c6;
            border-radius: 4px;
        }
        .notification p {
            margin: 0;
        }
        .admin-icon {
            position: fixed;
            top: 20px;
            right: 20px;
            font-size: 24px;
            cursor: pointer;
            color: #1b3b5f;
        }
        .admin-popup {
            display: none;
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: white;
            padding: 20px;
            border: 2px solid #ccc;
            border-radius: 8px;
            z-index: 100;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        .admin-popup input[type="password"] {
            width: 100%;
            padding: 10px;
            margin-top: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
        }
        .admin-popup button {
            margin-top: 10px;
            padding: 10px;
            background-color: #007bff;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            width: 100%;
        }
        .admin-popup button:hover {
            background-color: #0056b3;
        }
        .admin-section {
            display: none;
            margin-top: 20px;
        }
        table {
            width: 100%;
            border-collapse: collapse;
        }
        table, th, td {
            border: 1px solid #ddd;
            padding: 8px;
        }
        th {
            background-color: #f2f2f2;
        }
        td button {
            background-color: #d9534f;
            color: white;
            border: none;
            padding: 5px 10px;
            cursor: pointer;
        }
        td button:hover {
            background-color: #c9302c;
        }
        .count-section {
            margin-top: 20px;
            font-size: 16px;
            font-weight: bold;
        }
        .icon {
            margin-right: 5px;
        }
        .back-link {
            display: none;
            margin-top: 10px;
            color: #007bff;
            text-decoration: underline;
            cursor: pointer;
        }
        .popup-terms {
            display: none;
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            z-index: 101;
        }
        .popup-terms p {
            margin: 0;
        }
        .popup-terms button {
            margin-top: 10px;
            padding: 5px 10px;
            background-color: #007bff;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
    </style>
    <script>
        let registrants = JSON.parse(localStorage.getItem('registrants')) || [];
        let maxRegistrants = 10;

        window.onload = function() {
            updateTable();
            updateCounter();
        }

        function showNotification() {
            if (registrants.length < maxRegistrants) {
                let meno = document.getElementById("meno").value;
                let priezvisko = document.getElementById("priezvisko").value;
                let vek = document.getElementById("vek").value;
                let email = document.getElementById("email").value;
                let telefon = document.getElementById("telefon").value;
                let adresa = document.getElementById("adresa").value;
                let date = new Date();
                let registrationDate = date.toLocaleDateString() + " " + date.toLocaleTimeString();

                registrants.push({
                    meno,
                    priezvisko,
                    vek,
                    email,
                    telefon,
                    adresa,
                    registrationDate
                });

                localStorage.setItem('registrants', JSON.stringify(registrants));
                updateTable();
                updateCounter();
            }

            document.querySelector('form').style.display = 'none';
            document.querySelector('.notification').style.display = 'block';
            document.querySelector('.back-link').style.display = 'block';
        }

        function updateCounter() {
            document.getElementById('registrantCount').innerText = registrants.length;
        }

        function updateTable() {
            const tableBody = document.getElementById("registrantsTableBody");
            tableBody.innerHTML = "";

            registrants.forEach((registrant, index) => {
                let row = `<tr>
                            <td>${registrant.meno}</td>
                            <td>${registrant.priezvisko}</td>
                            <td>${registrant.vek}</td>
                            <td>${registrant.email}</td>
                            <td>${registrant.telefon}</td>
                            <td>${registrant.adresa}</td>
                            <td>${registrant.registrationDate}</td>
                            <td><button onclick="deleteRegistrant(${index})">Vymazať</button></td>
                           </tr>`;
                tableBody.innerHTML += row;
            });
        }

        function deleteRegistrant(index) {
            registrants.splice(index, 1);
            localStorage.setItem('registrants', JSON.stringify(registrants));
            updateTable();
            updateCounter();
        }

        function toggleAdminPopup() {
            document.querySelector('.admin-popup').style.display = 'block';
        }

        function closeAdminPopup() {
            document.querySelector('.admin-popup').style.display = 'none';
        }

        function loginAsAdmin() {
            const password = document.getElementById("adminPassword").value;
            if (password === "1420") {
                closeAdminPopup();
                document.querySelector('.admin-section').style.display = 'block';
            } else {
                alert("Nesprávne heslo!");
            }
        }

        function goBackToForm() {
            document.querySelector('form').style.display = 'block';
            document.querySelector('.notification').style.display = 'none';
            document.querySelector('.back-link').style.display = 'none';
        }

        function showTermsPopup() {
            document.querySelector('.popup-terms').style.display = 'block';
        }

        function closeTermsPopup() {
            document.querySelector('.popup-terms').style.display = 'none';
        }
    </script>
</head>
<body>

<div class="admin-icon" onclick="toggleAdminPopup()">👨‍💻</div>

<div class="admin-popup">
    <h2>Prihlásenie pre správcu</h2>
    <input type="password" id="adminPassword" placeholder="Zadajte heslo">
    <button onclick="loginAsAdmin()">Prihlásiť</button>
</div>

<div class="popup-terms">
    <h3>Všeobecné podmienky</h3>
    <p><strong> Podmienky účasti na workshope „Stojka na rukách“ </strong> <br>
<strong> Organizátor: </strong> Bc. Jaroslav Pásler, ďalej len „Organizátor“ <br>
<strong> Účastník: </strong> Osoba, ktorá sa prihlasuje na workshop „Stojka na rukách“, ďalej len „Účastník“ <br>
Účastník svojim zaškrtnutím súhlasíte s nasledujúcimi podmienkami: <br>
<strong> 1. Záväzky organizátora: </strong> Organizátor sa zaväzuje poskytnúť odborné vedenie a podporu počas trvania workshopu „Stojka na rukách“, pričom jeho cieľom je zabezpečiť čo najkvalitnejší priebeh pre účastníkov. <br>
<strong> 2. Povinnosti účastníka: </strong> Účastník sa zaväzuje: <br>
•	Dodržiavajte všetky pokyny Organizátora workshopu. <br>
•	Správať sa ohľaduplne a rešpektovať ostatných. <br>
•	Uvedomiť si svoje fyzické či iné obmedzenia, ktoré by mohli predstavovať riziko počas workshopu a včas o nich oboznámiť Organizátora. <br>
<strong> 3. Zodpovednosť za zranenie: </strong> Organizátor nenesie zodpovednosť za zranenie alebo úrazy, ktoré vznikli v dôsledku nedbanlivosti účastníka alebo nerešpektovania pokynov organizátora. <br>
<strong> 4. Zrušenie účasti: </strong> Účastník má právo opustiť svoju účasť na workshope minimálne 5 dní pred jeho začiatkom. V opačnom prípade nebude poplatok za workshop vrátený, avšak účastník môže za seba nájsť náhradu. <br>
<strong> 5. Platobné podmienky: </strong> Účastník sa zaväzuje uhradiť poplatok za workshop vopred podľa pokynov Organizátora. Nezaplatenie poplatku môže viesť k zrušeniu rezervácie miesta na workshope. Organizátor sa zaväzuje poslať Účastníkovi doklad o platbe. <br>
<strong> 6. Miesto a čas konania workshopu: </strong> Miesto konania workshopu je Yogaroom (Krupina, Obchodná 554/10, 963 01). Workshop sa uskutoční 16.11. 2024 od 9:00 do 11:30. <br>
<strong> 7. Ochrana osobných údajov: </strong> Účastník súhlasí s tým, že organizátor môže spracúvať jeho osobné údaje za účelom organizácie workshopu a komunikácie s účastníkom v súlade s platnými právnymi predpismi o ochrane osobných údajov. <br>
</p>
    <button onclick="closeTermsPopup()">Zatvoriť</button>
</div>

<div class="container">
    <h1>STOJKA NA RUKÁCH - začiatočníci</h1>
    
    <p>Workshop <strong> STOJKA NA RUKÁCH </strong> je určený pre úplných začiatočníkov. Prejdeme si základnú teóriu, ale hlavne praktické cvičenia a techniky na zlepšenie sily, flexibility a rovnováhy potrebné na zvládnutie stojky v priestore. Workshop bude prebiehať pod vedením skúseného a certifikovaného <a href="https://instagram.com/jaryk_on_hands" target="_blank">trénera</a>.</p>
    <p>Workshop sa uskutoční <strong> 16.11.2024 od 9:00 do 11:30 </strong> v priestoroch <strong> Yogaroom </strong> (Krupina, Obchodná 554/10) </p>
    <p> Bližšie informácie dostanete po registrácii na email.</p>
    <p class="price">Cena workshopu: 30€ </p>

    <form onsubmit="event.preventDefault(); showNotification();">
        <label for="meno"><span class="icon">👤</span> Meno:</label>
        <input type="text" id="meno" name="meno" required>

        <label for="priezvisko"><span class="icon">👤</span> Priezvisko:</label>
        <input type="text" id="priezvisko" name="priezvisko" required>

        <label for="vek"><span class="icon">🎂</span> Vek:</label>
        <input type="number" id="vek" name="vek" required>

        <label for="email"><span class="icon">✉️</span> Email:</label>
        <input type="email" id="email" name="email" required>

        <label for="telefon"><span class="icon">📞</span> Telefónne číslo:</label>
        <input type="tel" id="telefon" name="telefon" required>

        <label for="adresa"><span class="icon">🏠</span> Adresa:</label>
        <input type="text" id="adresa" name="adresa" required>

        <label><input type="checkbox" required> Súhlasím so spracovaním osobných údajov a <a href="javascript:void(0);" onclick="showTermsPopup()">všeobecnými podmienkami</a></label>

        <button type="submit">Prihlásiť sa</button>
    </form>

    <div class="notification">
        <p>Boli ste úspešne prihlásený! Informácie vám prídu do emailu do 24 hodín.</p>
        <span class="back-link" onclick="goBackToForm()">Chcete prihlásiť niekoho iného? Kliknite sem.</span>
    </div>

    <div class="count-section">
        Počet prihlásených: <span id="registrantCount">0</span> / 10
    </div>
</div>

<div class="container admin-section">
    <h2>Zoznam prihlásených</h2>
    <table>
        <thead>
            <tr>
                <th>Meno</th>
                <th>Priezvisko</th>
                <th>Vek</th>
                <th>Email</th>
                <th>Telefón</th>
                <th>Adresa</th>
                <th>Dátum a čas prihlásenia</th>
                <th>Akcia</th>
            </tr>
        </thead>
        <tbody id="registrantsTableBody">
            <!-- Zoznam prihlásených sa zobrazí tu -->
        </tbody>
    </table>
</div>

</body>
</html>

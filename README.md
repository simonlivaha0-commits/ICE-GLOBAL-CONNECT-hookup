<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ICE SPOT LOUNGE</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/qr-code-generator/1.4.4/qrcode.min.js">
    <style>
        /* General body & luxury black theme */
        body {
            margin: 0;
            font-family: 'Arial', sans-serif;
            background-color: #000;
            color: #fff;
        }

        header {
            text-align: center;
            padding: 2rem;
            font-size: 2.5rem;
            font-weight: bold;
            letter-spacing: 2px;
            color: #00ffff;
        }

        main {
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 2rem;
        }

        form {
            background-color: #111;
            padding: 2rem;
            border-radius: 12px;
            max-width: 500px;
            width: 100%;
            box-shadow: 0 0 20px #00ffff50;
        }

        input, select, button {
            width: 100%;
            padding: 0.8rem;
            margin: 0.5rem 0;
            border-radius: 8px;
            border: none;
            font-size: 1rem;
        }

        button {
            background-color: #00ffff;
            color: #000;
            font-weight: bold;
            cursor: pointer;
        }

        button:hover {
            background-color: #00cccc;
        }

        #qr-code {
            margin-top: 2rem;
        }

        footer {
            text-align: center;
            padding: 1rem;
            font-size: 0.9rem;
            color: #555;
        }

        /* Admin Dashboard */
        #admin-dashboard {
            display: none;
            background-color: #111;
            padding: 2rem;
            border-radius: 12px;
            box-shadow: 0 0 20px #00ffff50;
            max-width: 700px;
            margin-top: 2rem;
        }

        #admin-dashboard h2 {
            color: #ff00ff;
        }

        table {
            width: 100%;
            border-collapse: collapse;
        }

        th, td {
            padding: 0.5rem;
            border-bottom: 1px solid #333;
            text-align: left;
        }

        th {
            color: #00ffff;
        }
    </style>
</head>
<body>

<header>ICE SPOT LOUNGE</header>

<main>
    <!-- Booking Form -->
    <form id="bookingForm">
        <h2>Book Your Spot</h2>
        <input type="text" id="name" placeholder="Full Name" required>
        <input type="email" id="email" placeholder="Email" required>
        <input type="date" id="date" required>
        <select id="vip">
            <option value="standard">Standard</option>
            <option value="vip">VIP</option>
        </select>
        <button type="submit">Book Now</button>
    </form>

    <!-- QR Code Display -->
    <div id="qr-code"></div>

    <!-- Admin Dashboard Login -->
    <form id="adminLogin" style="margin-top:2rem; max-width:400px;">
        <h2>Admin Login</h2>
        <input type="text" id="adminId" placeholder="Admin ID" required>
        <input type="password" id="adminPass" placeholder="Password" required>
        <button type="submit">Login</button>
    </form>

    <div id="admin-dashboard">
        <h2>Admin Dashboard</h2>
        <table id="bookingTable">
            <thead>
                <tr>
                    <th>Name</th>
                    <th>Email</th>
                    <th>Date</th>
                    <th>VIP</th>
                </tr>
            </thead>
            <tbody></tbody>
        </table>
    </div>
</main>

<footer>© 2026 ICE SPOT LOUNGE | All Rights Reserved</footer>

<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/qrcode"></script>
<script>
    // Admin credentials (replace with secure backend later)
    const ADMIN_ID = "930472";
    const ADMIN_PASS = "#2@!7_";

    let bookings = [];

    // Limit booking to 3 months in advance
    $('#date').attr('min', new Date().toISOString().split('T')[0]);
    let maxDate = new Date();
    maxDate.setMonth(maxDate.getMonth() + 3);
    $('#date').attr('max', maxDate.toISOString().split('T')[0]);

    // Booking form submission
    $('#bookingForm').on('submit', function(e){
        e.preventDefault();
        const name = $('#name').val();
        const email = $('#email').val();
        const date = $('#date').val();
        const vip = $('#vip').val();

        const booking = { name, email, date, vip };
        bookings.push(booking);

        // Generate QR Code
        $('#qr-code').empty();
        new QRCode(document.getElementById("qr-code"), JSON.stringify(booking));

        alert("Booking successful! QR code generated.");
        this.reset();
    });

    // Admin Login
    $('#adminLogin').on('submit', function(e){
        e.preventDefault();
        const id = $('#adminId').val();
        const pass = $('#adminPass').val();
        if(id === ADMIN_ID && pass === ADMIN_PASS){
            $('#admin-dashboard').show();
            $('#adminLogin').hide();
            updateDashboard();
        } else {
            alert("Invalid credentials.");
        }
    });

    function updateDashboard() {
        const tbody = $('#bookingTable tbody');
        tbody.empty();
        bookings.forEach(b => {
            tbody.append(`<tr>
                <td>${b.name}</td>
                <td>${b.email}</td>
                <td>${b.date}</td>
                <td>${b.vip}</td>
            </tr>`);
        });
    }

    // Payment integration (PayPal)
    $('#bookingForm').append(`
        <form action="https://www.paypal.com/cgi-bin/webscr" method="post" target="_blank" style="display:none;" id="paypalForm">
            <input type="hidden" name="cmd" value="_xclick">
            <input type="hidden" name="business" value="jeemsimon@gmail.com">
            <input type="hidden" name="item_name" value="ICE SPOT LOUNGE Booking">
            <input type="hidden" name="amount" value="100">
            <input type="hidden" name="currency_code" value="USD">
            <input type="submit" value="Pay with PayPal">
        </form>
    `);
</script>

</body>
</html>

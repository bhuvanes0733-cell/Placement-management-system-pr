# Placement-management-system-pr
<!DOCTYPE html>
<html>
<head>
    <title>Placement Management System</title>
    <link rel="stylesheet" href="style.css">
</head>

<body>

<header>
    <h1>Placement Management System</h1>
    <p>Student Management Portal</p>
</header>

<div class="container">

    <h2>Add / Edit Student</h2>

    <form id="studentForm">

        <input type="hidden" id="studentId">

        <input
            type="text"
            id="name"
            placeholder="Student Name"
            required
        >

        <input
            type="email"
            id="email"
            placeholder="Email"
            required
        >

        <input
            type="text"
            id="department"
            placeholder="Department"
            required
        >

        <input
            type="number"
            id="cgpa"
            placeholder="CGPA"
            min="0"
            max="10"
            step="0.01"
            required
        >

        <input
            type="text"
            id="phone"
            placeholder="Phone Number"
            required
        >

        <button type="submit">Save Student</button>
        <button type="button" onclick="clearForm()">Clear</button>

    </form>

    <h2>Students</h2>

    <input
        type="text"
        id="search"
        placeholder="Search student..."
        onkeyup="searchStudents()"
    >

    <table>

        <thead>
            <tr>
                <th>ID</th>
                <th>Name</th>
                <th>Email</th>
                <th>Department</th>
                <th>CGPA</th>
                <th>Phone</th>
                <th>Actions</th>
            </tr>
        </thead>

        <tbody id="studentTable"></tbody>

    </table>

</div>

<script src="script.js"></script>

</body>
</html>

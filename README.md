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
* {
    box-sizing: border-box;
}

body {
    margin: 0;
    font-family: Arial, sans-serif;
    background: #f4f6f8;
}

header {
    background: #1f2937;
    color: white;
    text-align: center;
    padding: 25px;
}

.container {
    width: 90%;
    margin: 30px auto;
    background: white;
    padding: 25px;
    border-radius: 10px;
}

form {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 12px;
    margin-bottom: 30px;
}

input {
    padding: 12px;
    border: 1px solid #ccc;
    border-radius: 6px;
}

button {
    padding: 12px;
    border: none;
    border-radius: 6px;
    cursor: pointer;
    background: #2563eb;
    color: white;
}

button:hover {
    opacity: 0.85;
}

table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 20px;
}

th,
td {
    padding: 12px;
    border: 1px solid #ddd;
    text-align: center;
}

th {
    background: #1f2937;
    color: white;
}

.edit {
    background: #16a34a;
}

.delete {
    background: #dc2626;
}

#search {
    width: 100%;
    margin-top: 10px;
}

@media (max-width: 700px) {

    form {
        grid-template-columns: 1fr;
    }

    .container {
        width: 95%;
        overflow-x: auto;
    }

    table {
        min-width: 800px;
    }
}
const API_URL = "http://127.0.0.1:8000/api/students/";

let students = [];

document
    .getElementById("studentForm")
    .addEventListener("submit", saveStudent);

async function loadStudents() {

    try {

        const response = await fetch(API_URL);

        students = await response.json();

        displayStudents(students);

    } catch (error) {

        alert("Unable to connect to backend.");

    }
}

function displayStudents(data) {

    const table = document.getElementById("studentTable");

    table.innerHTML = "";

    data.forEach(student => {

        table.innerHTML += `
            <tr>

                <td>${student.id}</td>

                <td>${student.name}</td>

                <td>${student.email}</td>

                <td>${student.department}</td>

                <td>${student.cgpa}</td>

                <td>${student.phone}</td>

                <td>

                    <button
                        class="edit"
                        onclick="editStudent(${student.id})">
                        Edit
                    </button>

                    <button
                        class="delete"
                        onclick="deleteStudent(${student.id})">
                        Delete
                    </button>

                </td>

            </tr>
        `;
    });
}

async function saveStudent(event) {

    event.preventDefault();

    const id = document.getElementById("studentId").value;

    const student = {

        name: document.getElementById("name").value,

        email: document.getElementById("email").value,

        department: document.getElementById("department").value,

        cgpa: parseFloat(
            document.getElementById("cgpa").value
        ),

        phone: document.getElementById("phone").value
    };

    try {

        let response;

        if (id) {

            response = await fetch(API_URL + id + "/", {

                method: "PUT",

                headers: {
                    "Content-Type": "application/json"
                },

                body: JSON.stringify(student)

            });

        } else {

            response = await fetch(API_URL, {

                method: "POST",

                headers: {
                    "Content-Type": "application/json"
                },

                body: JSON.stringify(student)

            });
        }

        if (!response.ok) {

            const error = await response.json();

            alert(JSON.stringify(error));

            return;
        }

        alert(
            id
                ? "Student updated successfully!"
                : "Student added successfully!"
        );

        clearForm();

        loadStudents();

    } catch (error) {

        alert("Server connection failed.");

    }
}

function editStudent(id) {

    const student = students.find(s => s.id === id);

    if (!student) return;

    document.getElementById("studentId").value = student.id;

    document.getElementById("name").value = student.name;

    document.getElementById("email").value = student.email;

    document.getElementById("department").value =
        student.department;

    document.getElementById("cgpa").value = student.cgpa;

    document.getElementById("phone").value = student.phone;

    window.scrollTo({
        top: 0,
        behavior: "smooth"
    });
}

async function deleteStudent(id) {

    if (!confirm("Are you sure you want to delete this student?")) {
        return;
    }

    try {

        const response = await fetch(
            API_URL + id + "/",
            {
                method: "DELETE"
            }
        );

        if (response.ok) {

            alert("Student deleted successfully!");

            loadStudents();

        } else {

            alert("Unable to delete student.");

        }

    } catch (error) {

        alert("Server connection failed.");

    }
}

function clearForm() {

    document.getElementById("studentForm").reset();

    document.getElementById("studentId").value = "";

}

function searchStudents() {

    const keyword =
        document
            .getElementById("search")
            .value
            .toLowerCase();

    const filtered = students.filter(student =>

        student.name.toLowerCase().includes(keyword) ||

        student.email.toLowerCase().includes(keyword) ||

        student.department.toLowerCase().includes(keyword)

    );

    displayStudents(filtered);
}

loadStudents();

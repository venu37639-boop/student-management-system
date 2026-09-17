# student-management-system
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Student Management System</title>

    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: Arial, sans-serif;
        }

        body {
            background-color: #f2f4f7;
            padding: 30px;
        }

        .container {
            max-width: 1000px;
            margin: auto;
            background: white;
            padding: 25px;
            border-radius: 10px;
        }

        h1 {
            text-align: center;
            margin-bottom: 25px;
        }

        .form-box {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
            margin-bottom: 25px;
        }

        input, select {
            padding: 12px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }

        button {
            padding: 12px 18px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        .add-btn {
            background: #198754;
            color: white;
        }

        .search {
            width: 100%;
            margin-bottom: 20px;
        }

        table {
            width: 100%;
            border-collapse: collapse;
        }

        th, td {
            padding: 12px;
            border: 1px solid #ddd;
            text-align: center;
        }

        th {
            background: #333;
            color: white;
        }

        .edit-btn {
            background: #ffc107;
        }

        .delete-btn {
            background: #dc3545;
            color: white;
        }

        @media (max-width: 600px) {
            .form-box {
                grid-template-columns: 1fr;
            }

            table {
                font-size: 12px;
            }
        }
    </style>
</head>

<body>

<div class="container">

    <h1>Student Management System</h1>

    <!-- Student Form -->
    <div class="form-box">

        <input type="text" id="studentId" placeholder="Student ID">

        <input type="text" id="studentName" placeholder="Student Name">

        <input type="text" id="department" placeholder="Department">

        <select id="year">
            <option value="">Select Year</option>
            <option>1st Year</option>
            <option>2nd Year</option>
            <option>3rd Year</option>
            <option>4th Year</option>
        </select>

        <input type="email" id="email" placeholder="Email">

        <button class="add-btn" onclick="addStudent()">Add Student</button>

    </div>

    <!-- Search -->
    <input
        type="text"
        id="search"
        class="search"
        placeholder="Search by Student ID or Name..."
        onkeyup="displayStudents()"
    >

    <!-- Student Table -->
    <table>

        <thead>
            <tr>
                <th>ID</th>
                <th>Name</th>
                <th>Department</th>
                <th>Year</th>
                <th>Email</th>
                <th>Actions</th>
            </tr>
        </thead>

        <tbody id="studentTable">
        </tbody>

    </table>

</div>


<script>

    let students = [];
    let editIndex = -1;

    // Add Student
    function addStudent() {

        let id = document.getElementById("studentId").value;
        let name = document.getElementById("studentName").value;
        let department = document.getElementById("department").value;
        let year = document.getElementById("year").value;
        let email = document.getElementById("email").value;

        // Validation
        if (id === "" || name === "" || department === "" ||
            year === "" || email === "") {

            alert("Please fill all fields.");
            return;
        }

        let student = {
            id: id,
            name: name,
            department: department,
            year: year,
            email: email
        };

        // Update
        if (editIndex !== -1) {

            students[editIndex] = student;
            editIndex = -1;

        } else {

            // Check duplicate ID
            let exists = students.some(s => s.id === id);

            if (exists) {
                alert("Student ID already exists.");
                return;
            }

            students.push(student);
        }

        clearForm();
        displayStudents();
    }


    // Display Students
    function displayStudents() {

        let table = document.getElementById("studentTable");
        let search = document.getElementById("search").value.toLowerCase();

        table.innerHTML = "";

        students.forEach((student, index) => {

            if (
                student.id.toLowerCase().includes(search) ||
                student.name.toLowerCase().includes(search)
            ) {

                let row = `
                    <tr>

                        <td>${student.id}</td>

                        <td>${student.name}</td>

                        <td>${student.department}</td>

                        <td>${student.year}</td>

                        <td>${student.email}</td>

                        <td>
                            <button class="edit-btn"
                                onclick="editStudent(${index})">
                                Edit
                            </button>

                            <button class="delete-btn"
                                onclick="deleteStudent(${index})">
                                Delete
                            </button>
                        </td>

                    </tr>
                `;

                table.innerHTML += row;
            }

        });
    }


    // Edit Student
    function editStudent(index) {

        let student = students[index];

        document.getElementById("studentId").value = student.id;
        document.getElementById("studentName").value = student.name;
        document.getElementById("department").value = student.department;
        document.getElementById("year").value = student.year;
        document.getElementById("email").value = student.email;

        editIndex = index;
    }


    // Delete Student
    function deleteStudent(index) {

        if (confirm("Are you sure you want to delete this student?")) {

            students.splice(index, 1);

            displayStudents();
        }
    }


    // Clear Form
    function clearForm() {

        document.getElementById("studentId").value = "";
        document.getElementById("studentName").value = "";
        document.getElementById("department").value = "";
        document.getElementById("year").value = "";
        document.getElementById("email").value = "";
    }

</script>

</body>
</html>

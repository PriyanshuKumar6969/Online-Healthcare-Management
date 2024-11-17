# Online-Healthcare-Management
// Sample DAO class for Patients
public class PatientDAO {
    private Connection connect() {
        // JDBC connection
        String url = "jdbc:mysql://localhost:3306/hospital";
        String user = "root";
        String password = "password";
        Connection conn = null;
        try {
            conn = DriverManager.getConnection(url, user, password);
        } catch (SQLException e) {
            System.out.println(e.getMessage());
        }
        return conn;
    }

    public void addPatient(Patient patient) {
        String sql = "INSERT INTO Patients(name, dob, contact, address, medical_history) VALUES (?, ?, ?, ?, ?)";
        try (Connection conn = this.connect(); PreparedStatement pstmt = conn.prepareStatement(sql)) {
            pstmt.setString(1, patient.getName());
            pstmt.setDate(2, patient.getDob());
            pstmt.setString(3, patient.getContact());
            pstmt.setString(4, patient.getAddress());
            pstmt.setString(5, patient.getMedicalHistory());
            pstmt.executeUpdate();
        } catch (SQLException e) {
            System.out.println(e.getMessage());
        }
    }
}
@WebServlet("/registerPatient")
public class RegisterPatientServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String name = request.getParameter("name");
        Date dob = Date.valueOf(request.getParameter("dob"));
        String contact = request.getParameter("contact");
        String address = request.getParameter("address");
        String medicalHistory = request.getParameter("history");

        Patient patient = new Patient(name, dob, contact, address, medicalHistory);
        PatientDAO patientDAO = new PatientDAO();
        patientDAO.addPatient(patient);
        
        response.sendRedirect("success.html");
    }
}
<!-- Sample Patient Registration Form -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Patient Registration</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        form {
            width: 300px;
            margin: auto;
        }
        label, input {
            display: block;
            margin: 10px 0;
        }
    </style>
</head>
<body>
    <h2>Patient Registration</h2>
    <form action="registerPatient" method="post">
        <label for="name">Name:</label>
        <input type="text" id="name" name="name" required>
        
        <label for="dob">Date of Birth:</label>
        <input type="date" id="dob" name="dob" required>
        
        <label for="contact">Contact:</label>
        <input type="tel" id="contact" name="contact" required>
        
        <label for="address">Address:</label>
        <input type="text" id="address" name="address" required>
        
        <label for="history">Medical History:</label>
        <textarea id="history" name="history" rows="4"></textarea>
        
        <button type="submit">Register</button>
    </form>
</body>
</html>

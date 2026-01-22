# Employee-performance-tracker.
package com.example.employeeperformancetracker

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Surface
import androidx.compose.ui.Modifier
import com.example.employeeperformancetracker.navigation.AppNavGraph
import com.example.employeeperformancetracker.ui.theme.EmployeeperformancetrackerTheme

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            EmployeeperformancetrackerTheme {
                Surface(
                    modifier = Modifier.fillMaxSize(),
                    color = MaterialTheme.colorScheme.background
                ) {
                    AppNavGraph()
                }
            }
        }
    }
}

package com.example.employeeperformancetracker.ui.theme

import androidx.compose.ui.graphics.Color

val Purple80 = Color(0xFFD0BCFF)
val PurpleGrey80 = Color(0xFFCCC2DC)
val Pink80 = Color(0xFFEFB8C8)

val Purple40 = Color(0xFF6650a4)
val PurpleGrey40 = Color(0xFF625b71)
val Pink40 = Color(0xFF7D5260)

package com.example.employeeperformancetracker.ui.theme

import androidx.compose.material3.Typography
import androidx.compose.ui.text.TextStyle
import androidx.compose.ui.text.font.FontFamily
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.unit.sp

val Typography = Typography(
    bodyLarge = TextStyle(
        fontFamily = FontFamily.Default,
        fontWeight = FontWeight.Normal,
        fontSize = 16.sp,
        lineHeight = 24.sp,
        letterSpacing = 0.5.sp
    )
)

package com.example.employeeperformancetracker.ui.theme

import android.app.Activity
import android.os.Build
import androidx.compose.foundation.isSystemInDarkTheme
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.darkColorScheme
import androidx.compose.material3.dynamicDarkColorScheme
import androidx.compose.material3.dynamicLightColorScheme
import androidx.compose.material3.lightColorScheme
import androidx.compose.runtime.Composable
import androidx.compose.runtime.SideEffect
import androidx.compose.ui.graphics.toArgb
import androidx.compose.ui.platform.LocalContext
import androidx.compose.ui.platform.LocalView
import androidx.core.view.WindowCompat

private val DarkColorScheme = darkColorScheme(
    primary = Purple80,
    secondary = PurpleGrey80,
    tertiary = Pink80
)

private val LightColorScheme = lightColorScheme(
    primary = Purple40,
    secondary = PurpleGrey40,
    tertiary = Pink40
)

@Composable
fun EmployeeperformancetrackerTheme(
    darkTheme: Boolean = isSystemInDarkTheme(),
    dynamicColor: Boolean = true,
    content: @Composable () -> Unit
) {
    val colorScheme = when {
        dynamicColor && Build.VERSION.SDK_INT >= Build.VERSION_CODES.S -> {
            val context = LocalContext.current
            if (darkTheme) dynamicDarkColorScheme(context) else dynamicLightColorScheme(context)
        }
        darkTheme -> DarkColorScheme
        else -> LightColorScheme
    }
    val view = LocalView.current
    if (!view.isInEditMode) {
        SideEffect {
            val window = (view.context as Activity).window
            window.statusBarColor = colorScheme.primary.toArgb()
            WindowCompat.getInsetsController(window, view).isAppearanceLightStatusBars = darkTheme
        }
    }

    MaterialTheme(
        colorScheme = colorScheme,
        typography = Typography,
        content = content
    )
}

package com.example.employeeperformancetracker.navigation

import androidx.compose.runtime.Composable
import androidx.navigation.compose.NavHost
import androidx.navigation.compose.composable
import androidx.navigation.compose.rememberNavController
import com.example.employeeperformancetracker.ui.WelcomeScreen
import com.example.employeeperformancetracker.ui.LoginScreen
import com.example.employeeperformancetracker.ui.HRDashboardScreen
import com.example.employeeperformancetracker.ui.EmployeeDashboardScreen
import com.example.employeeperformancetracker.ui.EmployeeListScreen
import com.example.employeeperformancetracker.ui.AddEmployeeScreen
import com.example.employeeperformancetracker.ui.MarkAttendanceScreen
import com.example.employeeperformancetracker.ui.AttendanceListScreen
import com.example.employeeperformancetracker.ui.HRAnalyticsScreen

@Composable
fun AppNavGraph() {
    val navController = rememberNavController()

    NavHost(
        navController = navController,
        startDestination = "welcome"
    ) {
        composable("welcome") {
            WelcomeScreen(
                onNavigateToLogin = {
                    navController.navigate("login")
                }
            )
        }

        composable("login") {
            LoginScreen(
                onNavigateToHrDashboard = {
                    navController.navigate("hr_dashboard") {
                        popUpTo("welcome") { inclusive = true }
                    }
                },
                onNavigateToEmployeeDashboard = {
                    navController.navigate("employee_dashboard") {
                        popUpTo("welcome") { inclusive = true }
                    }
                }
            )
        }

        composable("hr_dashboard") {
            HRDashboardScreen(
                onNavigateToEmployeeList = {
                    navController.navigate("employee_list")
                },
                onNavigateToAddEmployee = {
                    navController.navigate("add_employee")
                },
                onNavigateToAttendanceList = {
                    navController.navigate("attendance_list")
                },
                onNavigateToAnalytics = {
                    navController.navigate("hr_analytics")
                }
            )
        }

        composable("employee_dashboard") {
            EmployeeDashboardScreen(
                onNavigateToMarkAttendance = {
                    navController.navigate("mark_attendance")
                },
                onNavigateToAttendanceList = {
                    navController.navigate("attendance_list")
                }
            )
        }

        composable("employee_list") {
            EmployeeListScreen(
                onNavigateBack = {
                    navController.popBackStack()
                }
            )
        }

        composable("add_employee") {
            AddEmployeeScreen(
                onNavigateBack = {
                    navController.popBackStack()
                }
            )
        }

        composable("mark_attendance") {
            MarkAttendanceScreen(
                onNavigateBack = {
                    navController.popBackStack()
                }
            )
        }

        composable("attendance_list") {
            AttendanceListScreen(
                onNavigateBack = {
                    navController.popBackStack()
                }
            )
        }

        composable("hr_analytics") {
            HRAnalyticsScreen(
                onNavigateBack = {
                    navController.popBackStack()
                }
            )
        }
    }
}

package com.example.employeeperformancetracker.ui

import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.Composable
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.text.style.TextAlign
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

@Composable
fun WelcomeScreen(onNavigateToLogin: () -> Unit) {
    Surface(
        modifier = Modifier.fillMaxSize(),
        color = MaterialTheme.colorScheme.background
    ) {
        Column(
            modifier = Modifier
                .fillMaxSize()
                .padding(24.dp),
            horizontalAlignment = Alignment.CenterHorizontally,
            verticalArrangement = Arrangement.Center
        ) {
            Text(
                text = "Employee Performance Tracker",
                fontSize = 32.sp,
                fontWeight = FontWeight.Bold,
                textAlign = TextAlign.Center,
                color = MaterialTheme.colorScheme.primary
            )

            Spacer(modifier = Modifier.height(16.dp))

            Text(
                text = "Manage and track employee performance efficiently",
                fontSize = 16.sp,
                textAlign = TextAlign.Center,
                color = MaterialTheme.colorScheme.onSurfaceVariant
            )

            Spacer(modifier = Modifier.height(48.dp))

            Button(
                onClick = onNavigateToLogin,
                modifier = Modifier
                    .fillMaxWidth()
                    .height(56.dp)
            ) {
                Text(
                    text = "Get Started",
                    fontSize = 18.sp
                )
            }
        }
    }
}

package com.example.employeeperformancetracker.ui

import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.text.input.PasswordVisualTransformation
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

@Composable
fun LoginScreen(
    onNavigateToHrDashboard: () -> Unit,
    onNavigateToEmployeeDashboard: () -> Unit
) {
    var email by remember { mutableStateOf("") }
    var password by remember { mutableStateOf("") }
    var selectedRole by remember { mutableStateOf("HR") }

    Surface(
        modifier = Modifier.fillMaxSize(),
        color = MaterialTheme.colorScheme.background
    ) {
        Column(
            modifier = Modifier
                .fillMaxSize()
                .padding(24.dp),
            horizontalAlignment = Alignment.CenterHorizontally,
            verticalArrangement = Arrangement.Center
        ) {
            Text(
                text = "Login",
                fontSize = 32.sp,
                fontWeight = FontWeight.Bold,
                color = MaterialTheme.colorScheme.primary
            )

            Spacer(modifier = Modifier.height(32.dp))

            OutlinedTextField(
                value = email,
                onValueChange = { email = it },
                label = { Text("Email") },
                modifier = Modifier.fillMaxWidth(),
                singleLine = true
            )

            Spacer(modifier = Modifier.height(16.dp))

            OutlinedTextField(
                value = password,
                onValueChange = { password = it },
                label = { Text("Password") },
                modifier = Modifier.fillMaxWidth(),
                visualTransformation = PasswordVisualTransformation(),
                singleLine = true
            )

            Spacer(modifier = Modifier.height(24.dp))

            Row(
                modifier = Modifier.fillMaxWidth(),
                horizontalArrangement = Arrangement.SpaceEvenly
            ) {
                Row(verticalAlignment = Alignment.CenterVertically) {
                    RadioButton(
                        selected = selectedRole == "HR",
                        onClick = { selectedRole = "HR" }
                    )
                    Text("HR", modifier = Modifier.padding(start = 4.dp))
                }

                Row(verticalAlignment = Alignment.CenterVertically) {
                    RadioButton(
                        selected = selectedRole == "Employee",
                        onClick = { selectedRole = "Employee" }
                    )
                    Text("Employee", modifier = Modifier.padding(start = 4.dp))
                }
            }

            Spacer(modifier = Modifier.height(32.dp))

            Button(
                onClick = {
                    if (selectedRole == "HR") {
                        onNavigateToHrDashboard()
                    } else {
                        onNavigateToEmployeeDashboard()
                    }
                },
                modifier = Modifier
                    .fillMaxWidth()
                    .height(56.dp)
            ) {
                Text(
                    text = "Login",
                    fontSize = 18.sp
                )
            }
        }
    }
}

package com.example.employeeperformancetracker.ui

import androidx.compose.foundation.layout.*
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.*
import androidx.compose.material3.*
import androidx.compose.runtime.Composable
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun HRDashboardScreen(
    onNavigateToEmployeeList: () -> Unit,
    onNavigateToAddEmployee: () -> Unit,
    onNavigateToAttendanceList: () -> Unit,
    onNavigateToAnalytics: () -> Unit
) {
    Scaffold(
        topBar = {
            TopAppBar(
                title = { Text("HR Dashboard") },
                colors = TopAppBarDefaults.topAppBarColors(
                    containerColor = MaterialTheme.colorScheme.primaryContainer,
                    titleContentColor = MaterialTheme.colorScheme.onPrimaryContainer
                )
            )
        }
    ) { padding ->
        Column(
            modifier = Modifier
                .fillMaxSize()
                .padding(padding)
                .padding(16.dp)
        ) {
            Text(
                text = "Welcome, HR Manager",
                fontSize = 24.sp,
                fontWeight = FontWeight.Bold,
                modifier = Modifier.padding(bottom = 24.dp)
            )

            DashboardCard(
                title = "Employee List",
                icon = Icons.Default.Person,
                description = "View and manage all employees",
                onClick = onNavigateToEmployeeList
            )

            Spacer(modifier = Modifier.height(16.dp))

            DashboardCard(
                title = "Add Employee",
                icon = Icons.Default.Add,
                description = "Add new employee to the system",
                onClick = onNavigateToAddEmployee
            )

            Spacer(modifier = Modifier.height(16.dp))

            DashboardCard(
                title = "Attendance Records",
                icon = Icons.Default.DateRange,
                description = "View attendance history",
                onClick = onNavigateToAttendanceList
            )

            Spacer(modifier = Modifier.height(16.dp))

            DashboardCard(
                title = "Analytics",
                icon = Icons.Default.Info,
                description = "View performance analytics",
                onClick = onNavigateToAnalytics
            )
        }
    }
}

@Composable
fun DashboardCard(
    title: String,
    icon: androidx.compose.ui.graphics.vector.ImageVector,
    description: String,
    onClick: () -> Unit
) {
    Card(
        modifier = Modifier
            .fillMaxWidth()
            .height(100.dp),
        onClick = onClick,
        elevation = CardDefaults.cardElevation(defaultElevation = 4.dp)
    ) {
        Row(
            modifier = Modifier
                .fillMaxSize()
                .padding(16.dp),
            verticalAlignment = Alignment.CenterVertically
        ) {
            Icon(
                imageVector = icon,
                contentDescription = null,
                modifier = Modifier.size(48.dp),
                tint = MaterialTheme.colorScheme.primary
            )

            Spacer(modifier = Modifier.width(16.dp))

            Column {
                Text(
                    text = title,
                    fontSize = 18.sp,
                    fontWeight = FontWeight.Bold
                )
                Text(
                    text = description,
                    fontSize = 14.sp,
                    color = MaterialTheme.colorScheme.onSurfaceVariant
                )
            }
        }
    }
}
package com.example.employeeperformancetracker.ui

import androidx.compose.foundation.layout.*
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.*
import androidx.compose.material3.*
import androidx.compose.runtime.Composable
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun EmployeeDashboardScreen(
    onNavigateToMarkAttendance: () -> Unit,
    onNavigateToAttendanceList: () -> Unit
) {
    Scaffold(
        topBar = {
            TopAppBar(
                title = { Text("Employee Dashboard") },
                colors = TopAppBarDefaults.topAppBarColors(
                    containerColor = MaterialTheme.colorScheme.primaryContainer,
                    titleContentColor = MaterialTheme.colorScheme.onPrimaryContainer
                )
            )
        }
    ) { padding ->
        Column(
            modifier = Modifier
                .fillMaxSize()
                .padding(padding)
                .padding(16.dp)
        ) {
            Text(
                text = "Welcome, Employee",
                fontSize = 24.sp,
                fontWeight = FontWeight.Bold,
                modifier = Modifier.padding(bottom = 24.dp)
            )

            Card(
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(bottom = 24.dp),
                elevation = CardDefaults.cardElevation(defaultElevation = 4.dp)
            ) {
                Column(
                    modifier = Modifier.padding(16.dp)
                ) {
                    Text(
                        text = "Your Performance",
                        fontSize = 18.sp,
                        fontWeight = FontWeight.Bold
                    )
                    Spacer(modifier = Modifier.height(8.dp))
                    Text("Attendance: 95%")
                    Text("Tasks Completed: 45")
                    Text("Performance Score: 8.5/10")
                }
            }

            DashboardCard(
                title = "Mark Attendance",
                icon = Icons.Default.Check,
                description = "Mark your daily attendance",
                onClick = onNavigateToMarkAttendance
            )

            Spacer(modifier = Modifier.height(16.dp))

            DashboardCard(
                title = "View Attendance History",
                icon = Icons.Default.DateRange,
                description = "Check your attendance records",
                onClick = onNavigateToAttendanceList
            )
        }
    }
}
package com.example.employeeperformancetracker.ui

import androidx.compose.foundation.layout.*
import androidx.compose.foundation.lazy.LazyColumn
import androidx.compose.foundation.lazy.items
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.ArrowBack
import androidx.compose.material.icons.filled.Person
import androidx.compose.material3.*
import androidx.compose.runtime.Composable
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

data class Employee(
    val id: Int,
    val name: String,
    val email: String,
    val department: String,
    val position: String
)

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun EmployeeListScreen(onNavigateBack: () -> Unit) {
    val employees = listOf(
        Employee(1, "John Doe", "john@example.com", "IT", "Developer"),
        Employee(2, "Jane Smith", "jane@example.com", "HR", "Manager"),
        Employee(3, "Mike Johnson", "mike@example.com", "Sales", "Executive"),
        Employee(4, "Sarah Williams", "sarah@example.com", "IT", "Designer"),
        Employee(5, "Tom Brown", "tom@example.com", "Finance", "Accountant")
    )

    Scaffold(
        topBar = {
            TopAppBar(
                title = { Text("Employee List") },
                navigationIcon = {
                    IconButton(onClick = onNavigateBack) {
                        Icon(Icons.Default.ArrowBack, contentDescription = "Back")
                    }
                },
                colors = TopAppBarDefaults.topAppBarColors(
                    containerColor = MaterialTheme.colorScheme.primaryContainer,
                    titleContentColor = MaterialTheme.colorScheme.onPrimaryContainer
                )
            )
        }
    ) { padding ->
        LazyColumn(
            modifier = Modifier
                .fillMaxSize()
                .padding(padding)
                .padding(16.dp)
        ) {
            items(employees) { employee ->
                EmployeeCard(employee)
                Spacer(modifier = Modifier.height(12.dp))
            }
        }
    }
}

@Composable
fun EmployeeCard(employee: Employee) {
    Card(
        modifier = Modifier.fillMaxWidth(),
        elevation = CardDefaults.cardElevation(defaultElevation = 4.dp)
    ) {
        Row(
            modifier = Modifier
                .fillMaxWidth()
                .padding(16.dp),
            verticalAlignment = Alignment.CenterVertically
        ) {
            Icon(
                imageVector = Icons.Default.Person,
                contentDescription = null,
                modifier = Modifier.size(48.dp),
                tint = MaterialTheme.colorScheme.primary
            )

            Spacer(modifier = Modifier.width(16.dp))

            Column {
                Text(
                    text = employee.name,
                    fontSize = 18.sp,
                    fontWeight = FontWeight.Bold
                )
                Text(
                    text = employee.email,
                    fontSize = 14.sp,
                    color = MaterialTheme.colorScheme.onSurfaceVariant
                )
                Text(
                    text = "${employee.position} - ${employee.department}",
                    fontSize = 14.sp,
                    color = MaterialTheme.colorScheme.onSurfaceVariant
                )
            }
        }
    }
}
package com.example.employeeperformancetracker.ui

import androidx.compose.foundation.layout.*
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.ArrowBack
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun AddEmployeeScreen(onNavigateBack: () -> Unit) {
    var name by remember { mutableStateOf("") }
    var email by remember { mutableStateOf("") }
    var department by remember { mutableStateOf("") }
    var position by remember { mutableStateOf("") }
    var phone by remember { mutableStateOf("") }
    var showSuccessDialog by remember { mutableStateOf(false) }

    Scaffold(
        topBar = {
            TopAppBar(
                title = { Text("Add Employee") },
                navigationIcon = {
                    IconButton(onClick = onNavigateBack) {
                        Icon(Icons.Default.ArrowBack, contentDescription = "Back")
                    }
                },
                colors = TopAppBarDefaults.topAppBarColors(
                    containerColor = MaterialTheme.colorScheme.primaryContainer,
                    titleContentColor = MaterialTheme.colorScheme.onPrimaryContainer
                )
            )
        }
    ) { padding ->
        Column(
            modifier = Modifier
                .fillMaxSize()
                .padding(padding)
                .padding(16.dp)
        ) {
            Text(
                text = "Employee Details",
                fontSize = 20.sp,
                fontWeight = FontWeight.Bold,
                modifier = Modifier.padding(bottom = 16.dp)
            )

            OutlinedTextField(
                value = name,
                onValueChange = { name = it },
                label = { Text("Full Name") },
                modifier = Modifier.fillMaxWidth(),
                singleLine = true
            )

            Spacer(modifier = Modifier.height(12.dp))

            OutlinedTextField(
                value = email,
                onValueChange = { email = it },
                label = { Text("Email") },
                modifier = Modifier.fillMaxWidth(),
                singleLine = true
            )

            Spacer(modifier = Modifier.height(12.dp))

            OutlinedTextField(
                value = phone,
                onValueChange = { phone = it },
                label = { Text("Phone Number") },
                modifier = Modifier.fillMaxWidth(),
                singleLine = true
            )

            Spacer(modifier = Modifier.height(12.dp))

            OutlinedTextField(
                value = department,
                onValueChange = { department = it },
                label = { Text("Department") },
                modifier = Modifier.fillMaxWidth(),
                singleLine = true
            )

            Spacer(modifier = Modifier.height(12.dp))

            OutlinedTextField(
                value = position,
                onValueChange = { position = it },
                label = { Text("Position") },
                modifier = Modifier.fillMaxWidth(),
                singleLine = true
            )

            Spacer(modifier = Modifier.height(24.dp))

            Button(
                onClick = { showSuccessDialog = true },
                modifier = Modifier
                    .fillMaxWidth()
                    .height(56.dp),
                enabled = name.isNotBlank() && email.isNotBlank()
            ) {
                Text(
                    text = "Add Employee",
                    fontSize = 18.sp
                )
            }
        }
    }

    if (showSuccessDialog) {
        AlertDialog(
            onDismissRequest = { showSuccessDialog = false },
            title = { Text("Success") },
            text = { Text("Employee added successfully!") },
            confirmButton = {
                TextButton(onClick = {
                    showSuccessDialog = false
                    onNavigateBack()
                }) {
                    Text("OK")
                }
            }
        )
    }
}
package com.example.employeeperformancetracker.ui

import androidx.compose.foundation.layout.*
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.ArrowBack
import androidx.compose.material.icons.filled.Check
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp
import java.text.SimpleDateFormat
import java.util.*

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun MarkAttendanceScreen(onNavigateBack: () -> Unit) {
    var selectedStatus by remember { mutableStateOf("Present") }
    var showSuccessDialog by remember { mutableStateOf(false) }
    val currentDate = remember {
        SimpleDateFormat("EEEE, MMMM dd, yyyy", Locale.getDefault()).format(Date())
    }
    val currentTime = remember {
        SimpleDateFormat("hh:mm a", Locale.getDefault()).format(Date())
    }

    Scaffold(
        topBar = {
            TopAppBar(
                title = { Text("Mark Attendance") },
                navigationIcon = {
                    IconButton(onClick = onNavigateBack) {
                        Icon(Icons.Default.ArrowBack, contentDescription = "Back")
                    }
                },
                colors = TopAppBarDefaults.topAppBarColors(
                    containerColor = MaterialTheme.colorScheme.primaryContainer,
                    titleContentColor = MaterialTheme.colorScheme.onPrimaryContainer
                )
            )
        }
    ) { padding ->
        Column(
            modifier = Modifier
                .fillMaxSize()
                .padding(padding)
                .padding(16.dp),
            horizontalAlignment = Alignment.CenterHorizontally
        ) {
            Card(
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(bottom = 24.dp),
                elevation = CardDefaults.cardElevation(defaultElevation = 4.dp)
            ) {
                Column(
                    modifier = Modifier.padding(16.dp),
                    horizontalAlignment = Alignment.CenterHorizontally
                ) {
                    Text(
                        text = currentDate,
                        fontSize = 18.sp,
                        fontWeight = FontWeight.Bold
                    )
                    Spacer(modifier = Modifier.height(8.dp))
                    Text(
                        text = currentTime,
                        fontSize = 32.sp,
                        fontWeight = FontWeight.Bold,
                        color = MaterialTheme.colorScheme.primary
                    )
                }
            }

            Text(
                text = "Select Attendance Status",
                fontSize = 20.sp,
                fontWeight = FontWeight.Bold,
                modifier = Modifier.padding(bottom = 16.dp)
            )

            Column(
                modifier = Modifier.fillMaxWidth()
            ) {
                AttendanceStatusOption(
                    status = "Present",
                    selected = selectedStatus == "Present",
                    onSelect = { selectedStatus = "Present" }
                )

                Spacer(modifier = Modifier.height(12.dp))

                AttendanceStatusOption(
                    status = "Absent",
                    selected = selectedStatus == "Absent",
                    onSelect = { selectedStatus = "Absent" }
                )

                Spacer(modifier = Modifier.height(12.dp))

                AttendanceStatusOption(
                    status = "Half Day",
                    selected = selectedStatus == "Half Day",
                    onSelect = { selectedStatus = "Half Day" }
                )

                Spacer(modifier = Modifier.height(12.dp))

                AttendanceStatusOption(
                    status = "Work From Home",
                    selected = selectedStatus == "Work From Home",
                    onSelect = { selectedStatus = "Work From Home" }
                )
            }

            Spacer(modifier = Modifier.height(32.dp))

            Button(
                onClick = { showSuccessDialog = true },
                modifier = Modifier
                    .fillMaxWidth()
                    .height(56.dp)
            ) {
                Icon(
                    imageVector = Icons.Default.Check,
                    contentDescription = null,
                    modifier = Modifier.size(24.dp)
                )
                Spacer(modifier = Modifier.width(8.dp))
                Text(
                    text = "Submit Attendance",
                    fontSize = 18.sp
                )
            }
        }
    }

    if (showSuccessDialog) {
        AlertDialog(
            onDismissRequest = { showSuccessDialog = false },
            title = { Text("Success") },
            text = { Text("Attendance marked as $selectedStatus") },
            confirmButton = {
                TextButton(onClick = {
                    showSuccessDialog = false
                    onNavigateBack()
                }) {
                    Text("OK")
                }
            }
        )
    }
}

@Composable
fun AttendanceStatusOption(
    status: String,
    selected: Boolean,
    onSelect: () -> Unit
) {
    Card(
        modifier = Modifier.fillMaxWidth(),
        onClick = onSelect,
        colors = if (selected) {
            CardDefaults.cardColors(
                containerColor = MaterialTheme.colorScheme.primaryContainer
            )
        } else {
            CardDefaults.cardColors()
        }
    ) {
        Row(
            modifier = Modifier
                .fillMaxWidth()
                .padding(16.dp),
            verticalAlignment = Alignment.CenterVertically
        ) {
            RadioButton(
                selected = selected,
                onClick = onSelect
            )
            Spacer(modifier = Modifier.width(8.dp))
            Text(
                text = status,
                fontSize = 16.sp,
                fontWeight = if (selected) FontWeight.Bold else FontWeight.Normal
            )
        }
    }
}
package com.example.employeeperformancetracker.ui

import androidx.compose.foundation.layout.*
import androidx.compose.foundation.lazy.LazyColumn
import androidx.compose.foundation.lazy.items
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.ArrowBack
import androidx.compose.material3.*
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

data class AttendanceRecord(
    val id: Int,
    val date: String,
    val status: String,
    val checkIn: String,
    val checkOut: String
)

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun AttendanceListScreen(onNavigateBack: () -> Unit) {
    val attendanceRecords = listOf(
        AttendanceRecord(1, "Jan 07, 2026", "Present", "09:00 AM", "06:00 PM"),
        AttendanceRecord(2, "Jan 06, 2026", "Present", "09:15 AM", "06:10 PM"),
        AttendanceRecord(3, "Jan 05, 2026", "Work From Home", "09:00 AM", "05:45 PM"),
        AttendanceRecord(4, "Jan 04, 2026", "Present", "08:55 AM", "06:00 PM"),
        AttendanceRecord(5, "Jan 03, 2026", "Half Day", "09:00 AM", "01:00 PM"),
        AttendanceRecord(6, "Jan 02, 2026", "Present", "09:05 AM", "06:15 PM"),
        AttendanceRecord(7, "Jan 01, 2026", "Absent", "-", "-")
    )

    Scaffold(
        topBar = {
            TopAppBar(
                title = { Text("Attendance History") },
                navigationIcon = {
                    IconButton(onClick = onNavigateBack) {
                        Icon(Icons.Default.ArrowBack, contentDescription = "Back")
                    }
                },
                colors = TopAppBarDefaults.topAppBarColors(
                    containerColor = MaterialTheme.colorScheme.primaryContainer,
                    titleContentColor = MaterialTheme.colorScheme.onPrimaryContainer
                )
            )
        }
    ) { padding ->
        Column(
            modifier = Modifier
                .fillMaxSize()
                .padding(padding)
        ) {
            Card(
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(16.dp),
                elevation = CardDefaults.cardElevation(defaultElevation = 4.dp)
            ) {
                Column(
                    modifier = Modifier.padding(16.dp)
                ) {
                    Text(
                        text = "This Month Summary",
                        fontSize = 18.sp,
                        fontWeight = FontWeight.Bold
                    )
                    Spacer(modifier = Modifier.height(8.dp))
                    Row(
                        modifier = Modifier.fillMaxWidth(),
                        horizontalArrangement = Arrangement.SpaceBetween
                    ) {
                        SummaryItem("Present", "5")
                        SummaryItem("Absent", "1")
                        SummaryItem("WFH", "1")
                    }
                }
            }

            LazyColumn(
                modifier = Modifier.padding(horizontal = 16.dp)
            ) {
                items(attendanceRecords) { record ->
                    AttendanceRecordCard(record)
                    Spacer(modifier = Modifier.height(12.dp))
                }
            }
        }
    }
}

@Composable
fun SummaryItem(label: String, value: String) {
    Column {
        Text(
            text = value,
            fontSize = 24.sp,
            fontWeight = FontWeight.Bold,
            color = MaterialTheme.colorScheme.primary
        )
        Text(
            text = label,
            fontSize = 14.sp,
            color = MaterialTheme.colorScheme.onSurfaceVariant
        )
    }
}

@Composable
fun AttendanceRecordCard(record: AttendanceRecord) {
    Card(
        modifier = Modifier.fillMaxWidth(),
        elevation = CardDefaults.cardElevation(defaultElevation = 2.dp)
    ) {
        Column(
            modifier = Modifier
                .fillMaxWidth()
                .padding(16.dp)
        ) {
            Row(
                modifier = Modifier.fillMaxWidth(),
                horizontalArrangement = Arrangement.SpaceBetween
            ) {
                Text(
                    text = record.date,
                    fontSize = 16.sp,
                    fontWeight = FontWeight.Bold
                )
                StatusChip(record.status)
            }

            if (record.status != "Absent") {
                Spacer(modifier = Modifier.height(8.dp))
                Row(
                    modifier = Modifier.fillMaxWidth(),
                    horizontalArrangement = Arrangement.SpaceBetween
                ) {
                    Column {
                        Text(
                            text = "Check In",
                            fontSize = 12.sp,
                            color = MaterialTheme.colorScheme.onSurfaceVariant
                        )
                        Text(
                            text = record.checkIn,
                            fontSize = 14.sp,
                            fontWeight = FontWeight.Medium
                        )
                    }
                    Column {
                        Text(
                            text = "Check Out",
                            fontSize = 12.sp,
                            color = MaterialTheme.colorScheme.onSurfaceVariant
                        )
                        Text(
                            text = record.checkOut,
                            fontSize = 14.sp,
                            fontWeight = FontWeight.Medium
                        )
                    }
                }
            }
        }
    }
}

@Composable
fun StatusChip(status: String) {
    val color = when (status) {
        "Present" -> MaterialTheme.colorScheme.primary
        "Absent" -> MaterialTheme.colorScheme.error
        "Half Day" -> MaterialTheme.colorScheme.tertiary
        "Work From Home" -> MaterialTheme.colorScheme.secondary
        else -> MaterialTheme.colorScheme.outline
    }

    Surface(
        color = color.copy(alpha = 0.2f),
        shape = MaterialTheme.shapes.small
    ) {
        Text(
            text = status,
            modifier = Modifier.padding(horizontal = 12.dp, vertical = 4.dp),
            fontSize = 12.sp,
            fontWeight = FontWeight.Medium,
            color = color
        )
    }
}
package com.example.employeeperformancetracker.ui

import androidx.compose.foundation.layout.*
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.ArrowBack
import androidx.compose.material3.*
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun HRAnalyticsScreen(onNavigateBack: () -> Unit) {
    Scaffold(
        topBar = {
            TopAppBar(
                title = { Text("HR Analytics") },
                navigationIcon = {
                    IconButton(onClick = onNavigateBack) {
                        Icon(Icons.Default.ArrowBack, contentDescription = "Back")
                    }
                },
                colors = TopAppBarDefaults.topAppBarColors(
                    containerColor = MaterialTheme.colorScheme.primaryContainer,
                    titleContentColor = MaterialTheme.colorScheme.onPrimaryContainer
                )
            )
        }
    ) { padding ->
        Column(
            modifier = Modifier
                .fillMaxSize()
                .padding(padding)
                .padding(16.dp)
        ) {
            Text(
                text = "Performance Overview",
                fontSize = 20.sp,
                fontWeight = FontWeight.Bold,
                modifier = Modifier.padding(bottom = 16.dp)
            )

            Card(
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(bottom = 16.dp),
                elevation = CardDefaults.cardElevation(defaultElevation = 4.dp)
            ) {
                Column(
                    modifier = Modifier.padding(16.dp)
                ) {
                    Text(
                        text = "Overall Statistics",
                        fontSize = 18.sp,
                        fontWeight = FontWeight.Bold
                    )
                    Spacer(modifier = Modifier.height(12.dp))
                    AnalyticsRow("Total Employees", "45")
                    AnalyticsRow("Average Attendance", "92.5%")
                    AnalyticsRow("Average Performance", "8.2/10")
                }
            }

            Card(
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(bottom = 16.dp),
                elevation = CardDefaults.cardElevation(defaultElevation = 4.dp)
            ) {
                Column(
                    modifier = Modifier.padding(16.dp)
                ) {
                    Text(
                        text = "Department Performance",
                        fontSize = 18.sp,
                        fontWeight = FontWeight.Bold
                    )
                    Spacer(modifier = Modifier.height(12.dp))
                    DepartmentRow("IT", "8.5/10", "95%")
                    DepartmentRow("HR", "8.0/10", "90%")
                    DepartmentRow("Sales", "7.8/10", "88%")
                    DepartmentRow("Finance", "8.3/10", "93%")
                }
            }

            Card(
                modifier = Modifier.fillMaxWidth(),
                elevation = CardDefaults.cardElevation(defaultElevation = 4.dp)
            ) {
                Column(
                    modifier = Modifier.padding(16.dp)
                ) {
                    Text(
                        text = "Monthly Trends",
                        fontSize = 18.sp,
                        fontWeight = FontWeight.Bold
                    )
                    Spacer(modifier = Modifier.height(12.dp))
                    Text("This Month: 92.5% attendance")
                    Text("Last Month: 89.3% attendance")
                    Text("Improvement: +3.2%")
                }
            }
        }
    }
}

@Composable
fun AnalyticsRow(label: String, value: String) {
    Row(
        modifier = Modifier
            .fillMaxWidth()
            .padding(vertical = 4.dp),
        horizontalArrangement = Arrangement.SpaceBetween
    ) {
        Text(
            text = label,
            fontSize = 14.sp,
            color = MaterialTheme.colorScheme.onSurfaceVariant
        )
        Text(
            text = value,
            fontSize = 14.sp,
            fontWeight = FontWeight.Bold,
            color = MaterialTheme.colorScheme.primary
        )
    }
}

@Composable
fun DepartmentRow(dept: String, performance: String, attendance: String) {
    Column(
        modifier = Modifier
            .fillMaxWidth()
            .padding(vertical = 8.dp)
    ) {
        Text(
            text = dept,
            fontSize = 16.sp,
            fontWeight = FontWeight.Bold
        )
        Spacer(modifier = Modifier.height(4.dp))
        Row(
            modifier = Modifier.fillMaxWidth(),
            horizontalArrangement = Arrangement.SpaceBetween
        ) {
            Text(
                text = "Performance: $performance",
                fontSize = 14.sp,
                color = MaterialTheme.colorScheme.onSurfaceVariant
            )
            Text(
                text = "Attendance: $attendance",
                fontSize = 14.sp,
                color = MaterialTheme.colorScheme.onSurfaceVariant
            )
        }
        Spacer(modifier = Modifier.height(4.dp))
        Divider()
    }
}
package com.example.ept

import androidx.compose.material3.*
import androidx.compose.runtime.Composable
import androidx.navigation.NavController
import com.google.firebase.auth.FirebaseAuth

@Composable
fun LogoutButton(navController: NavController) {

    Button(
        onClick = {
            FirebaseAuth.getInstance().signOut()
            navController.navigate("welcome") {
                popUpTo(0) { inclusive = true }
            }
        }
    ) {
        Text("Logout")
    }
}
package com.example.employeeperformancetracker.ui

import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.text.input.PasswordVisualTransformation
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

@Composable
fun LoginScreen(
    onNavigateToHrDashboard: () -> Unit,
    onNavigateToEmployeeDashboard: () -> Unit
) {
    var email by remember { mutableStateOf("") }
    var password by remember { mutableStateOf("") }
    var selectedRole by remember { mutableStateOf("HR") }
    var errorMessage by remember { mutableStateOf("") }
    var showErrorDialog by remember { mutableStateOf(false) }

    Surface(
        modifier = Modifier.fillMaxSize(),
        color = MaterialTheme.colorScheme.background
    ) {
        Column(
            modifier = Modifier
                .fillMaxSize()
                .padding(24.dp),
            horizontalAlignment = Alignment.CenterHorizontally,
            verticalArrangement = Arrangement.Center
        ) {
            Text(
                text = "Login",
                fontSize = 32.sp,
                fontWeight = FontWeight.Bold,
                color = MaterialTheme.colorScheme.primary
            )

            Spacer(modifier = Modifier.height(32.dp))

            OutlinedTextField(
                value = email,
                onValueChange = { email = it },
                label = { Text("Email") },
                modifier = Modifier.fillMaxWidth(),
                singleLine = true,
                isError = errorMessage.isNotEmpty()
            )

            Spacer(modifier = Modifier.height(16.dp))

            OutlinedTextField(
                value = password,
                onValueChange = { password = it },
                label = { Text("Password") },
                modifier = Modifier.fillMaxWidth(),
                visualTransformation = PasswordVisualTransformation(),
                singleLine = true,
                isError = errorMessage.isNotEmpty()
            )

            if (errorMessage.isNotEmpty()) {
                Spacer(modifier = Modifier.height(8.dp))
                Text(
                    text = errorMessage,
                    color = MaterialTheme.colorScheme.error,
                    fontSize = 14.sp
                )
            }

            Spacer(modifier = Modifier.height(24.dp))

            Row(
                modifier = Modifier.fillMaxWidth(),
                horizontalArrangement = Arrangement.SpaceEvenly
            ) {
                Row(verticalAlignment = Alignment.CenterVertically) {
                    RadioButton(
                        selected = selectedRole == "HR",
                        onClick = {
                            selectedRole = "HR"
                            errorMessage = ""
                        }
                    )
                    Text("HR", modifier = Modifier.padding(start = 4.dp))
                }

                Row(verticalAlignment = Alignment.CenterVertically) {
                    RadioButton(
                        selected = selectedRole == "Employee",
                        onClick = {
                            selectedRole = "Employee"
                            errorMessage = ""
                        }
                    )
                    Text("Employee", modifier = Modifier.padding(start = 4.dp))
                }
            }

            Spacer(modifier = Modifier.height(32.dp))

            Button(
                onClick = {
                    // Validate credentials
                    val isValid = when (selectedRole) {
                        "HR" -> email == "hr@company.com" && password == "hr123"
                        "Employee" -> email == "emp@company.com" && password == "emp123"
                        else -> false
                    }

                    if (email.isEmpty() || password.isEmpty()) {
                        errorMessage = "Please enter email and password"
                    } else if (isValid) {
                        errorMessage = ""
                        if (selectedRole == "HR") {
                            onNavigateToHrDashboard()
                        } else {
                            onNavigateToEmployeeDashboard()
                        }
                    } else {
                        errorMessage = "Invalid credentials for $selectedRole"
                        showErrorDialog = true
                    }
                },
                modifier = Modifier
                    .fillMaxWidth()
                    .height(56.dp)
            ) {
                Text(
                    text = "Login",
                    fontSize = 18.sp
                )
            }

            Spacer(modifier = Modifier.height(24.dp))

            // Show credentials hint
            Card(
                modifier = Modifier.fillMaxWidth(),
                colors = CardDefaults.cardColors(
                    containerColor = MaterialTheme.colorScheme.primaryContainer
                )
            ) {
                Column(
                    modifier = Modifier.padding(16.dp)
                ) {
                    Text(
                        text = "Demo Credentials:",
                        fontWeight = FontWeight.Bold,
                        fontSize = 14.sp
                    )
                    Spacer(modifier = Modifier.height(8.dp))
                    Text("HR: hr@company.com / hr123", fontSize = 12.sp)
                    Text("Employee: emp@company.com / emp123", fontSize = 12.sp)
                }
            }
        }
    }

    if (showErrorDialog) {
        AlertDialog(
            onDismissRequest = { showErrorDialog = false },
            title = { Text("Login Failed") },
            text = { Text(errorMessage) },
            confirmButton = {
                TextButton(onClick = { showErrorDialog = false }) {
                    Text("OK")
                }
            }
        )
    }
}
package com.example.employeeperformancetracker

import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp
import androidx.navigation.NavController

@Composable
fun HrDashboardScreen(navController: NavController) {

    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp)
    ) {

        Text(
            text = "HR Dashboard",
            style = MaterialTheme.typography.headlineMedium
        )

        Spacer(modifier = Modifier.height(16.dp))

        Button(
            onClick = { navController.navigate("employee_list") },
            modifier = Modifier.fillMaxWidth()
        ) {
            Text("Manage Employees")
        }

        Spacer(modifier = Modifier.height(12.dp))

        Button(
            onClick = { navController.navigate("mark_attendance") },
            modifier = Modifier.fillMaxWidth()
        ) {
            Text("Mark Attendance")
        }

        Spacer(modifier = Modifier.height(12.dp))

        Button(
            onClick = { navController.navigate("analytics") },
            modifier = Modifier.fillMaxWidth()
        ) {
            Text("View Analytics")
        }

        Spacer(modifier = Modifier.height(24.dp))

        LogoutButton(navController)
    }
}
package com.example.employeeperformancetracker

import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.Composable
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp
import androidx.navigation.NavController

@Composable
fun EmployeeDashboardScreen(navController: NavController) {

    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp)
    ) {

        Text(
            text = "Employee Dashboard",
            style = MaterialTheme.typography.headlineMedium
        )

        Spacer(modifier = Modifier.height(16.dp))

        Button(
            onClick = {
                navController.navigate("attendance_list")
            }
        ) {
            Text("View Attendance")
        }

        Spacer(modifier = Modifier.height(24.dp))

        LogoutButton(navController)
    }
}
package com.example.employeeperformancetracker

package com.example.ept

data class Employee(
    val id: String = "",
    val name: String = "",
    val email: String = "",
    val department: String = "",
    val rating: Int = 0
)
package com.example.ept

data class Attendance(
    val id: String = "",
    val employeeId: String = "",
    val name: String = "",
    val date: String = "",
    val status: String = ""
)
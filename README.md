# GoldApp
import 'package:cloud_firestore/cloud_firestore.dart';
import 'package:flutter/material.dart';
import 'package:firebase_core/firebase_core.dart';
import 'package:firebase_auth/firebase_auth.dart';
import 'dart:async';
import 'dart:io';
import 'package:firebase_storage/firebase_storage.dart';
import 'package:image_picker/image_picker.dart';
import 'package:uuid/uuid.dart';
import 'dart:typed_data';
import 'package:image_picker_web/image_picker_web.dart';
import 'package:intl/intl.dart';
import 'package:flutter/foundation.dart' as Foundation;
import 'package:upi_india/upi_india.dart';

class GlobalData {
  static String? userEmail;
}

class UserSession {
  static String? email;

  static void setEmail(String userEmail) {
    email = userEmail;
  }

  static String? getEmail() {
    return email;
  }

  static void clear() {
    email = null;
  }
}

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp(
      options: FirebaseOptions(
          apiKey: "AIzaSyAH5rTugH6aOSWNcPAIkfmc7_c6e5Nzn4U",
          authDomain: "bhumobileapp.firebaseapp.com",
          projectId: "bhumobileapp",
          storageBucket: "bhumobileapp.firebasestorage.app",
          messagingSenderId: "45494962654",
          appId: "1:45494962654:web:fefa9b7781f6aed18e354f",
          measurementId: "G-7PZBX9YC9X"));
  runApp(MaterialApp(home: LoginPage()));
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Login Page',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: LoginPage(),
    );
  }
}

class UserForm extends StatefulWidget {
  @override
  _UserFormState createState() => _UserFormState();
}

class _UserFormState extends State<UserForm> {
  final _formKey = GlobalKey<FormState>();

  final nameController = TextEditingController();
  final numberController = TextEditingController();
  final addressController = TextEditingController();
  final aadharController = TextEditingController();
  final panController = TextEditingController();
  final emailController = TextEditingController();
  final passwordController = TextEditingController();
  final confirmPasswordController = TextEditingController();

  bool _isLoading = false;
  bool _obscurePassword = true;
  bool _obscureConfirmPassword = true;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(
          "Create Account",
          style: TextStyle(color: Colors.white),
        ),
        backgroundColor: const Color.fromARGB(255, 0, 0, 0),
        foregroundColor: Colors.white,
        elevation: 0,
      ),
      body: Container(
        decoration: BoxDecoration(
          image: DecorationImage(
            image: AssetImage('assets/images/1.png'),
            fit: BoxFit.cover,
          ),
        ),
        child: SafeArea(
          child: SingleChildScrollView(
            padding: EdgeInsets.all(20),
            child: Form(
              key: _formKey,
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.stretch,
                children: [
                  // Page title
                  Text(
                    "Register for Gold App",
                    style: TextStyle(
                      fontSize: 24,
                      fontWeight: FontWeight.bold,
                      color: const Color.fromARGB(255, 191, 89, 0),
                    ),
                    textAlign: TextAlign.center,
                  ),
                  SizedBox(height: 16),
                  Text(
                    "Please fill in your details",
                    textAlign: TextAlign.center,
                    style: TextStyle(
                      fontSize: 16,
                      color: Colors.grey[700],
                    ),
                  ),
                  SizedBox(height: 30),

                  // Full Name
                  TextFormField(
                    controller: nameController,
                    decoration: InputDecoration(
                      labelText: 'Full Name',
                      prefixIcon: Icon(Icons.person),
                      border: OutlineInputBorder(
                        borderRadius: BorderRadius.circular(12),
                      ),
                      filled: true,
                      fillColor: Colors.white,
                    ),
                    validator: (value) => value == null || value.isEmpty
                        ? 'Please enter your name'
                        : null,
                  ),
                  SizedBox(height: 16),

                  // Phone Number
                  TextFormField(
                    controller: numberController,
                    decoration: InputDecoration(
                      labelText: 'Phone Number',
                      prefixIcon: Icon(Icons.phone),
                      border: OutlineInputBorder(
                        borderRadius: BorderRadius.circular(12),
                      ),
                      filled: true,
                      fillColor: Colors.white,
                    ),
                    keyboardType: TextInputType.phone,
                    validator: (value) => value == null || value.isEmpty
                        ? 'Please enter your phone number'
                        : null,
                  ),
                  SizedBox(height: 16),

                  // Email Address
                  TextFormField(
                    controller: emailController,
                    decoration: InputDecoration(
                      labelText: 'Email Address',
                      prefixIcon: Icon(Icons.email),
                      border: OutlineInputBorder(
                        borderRadius: BorderRadius.circular(12),
                      ),
                      filled: true,
                      fillColor: Colors.white,
                    ),
                    keyboardType: TextInputType.emailAddress,
                    validator: (value) =>
                        value == null || value.isEmpty || !value.contains('@')
                            ? 'Please enter a valid email'
                            : null,
                  ),
                  SizedBox(height: 16),

                  // Password
                  TextFormField(
                    controller: passwordController,
                    obscureText: _obscurePassword,
                    decoration: InputDecoration(
                      labelText: 'Password',
                      prefixIcon: Icon(Icons.lock),
                      suffixIcon: IconButton(
                        icon: Icon(
                          _obscurePassword
                              ? Icons.visibility
                              : Icons.visibility_off,
                        ),
                        onPressed: () {
                          setState(() {
                            _obscurePassword = !_obscurePassword;
                          });
                        },
                      ),
                      border: OutlineInputBorder(
                        borderRadius: BorderRadius.circular(12),
                      ),
                      filled: true,
                      fillColor: Colors.white,
                    ),
                    validator: (value) {
                      if (value == null || value.isEmpty) {
                        return 'Please enter a password';
                      }
                      if (value.length < 6) {
                        return 'Password must be at least 6 characters long';
                      }
                      return null;
                    },
                  ),
                  SizedBox(height: 16),

                  // Confirm Password
                  TextFormField(
                    controller: confirmPasswordController,
                    obscureText: _obscureConfirmPassword,
                    decoration: InputDecoration(
                      labelText: 'Confirm Password',
                      prefixIcon: Icon(Icons.lock_outline),
                      suffixIcon: IconButton(
                        icon: Icon(
                          _obscureConfirmPassword
                              ? Icons.visibility
                              : Icons.visibility_off,
                        ),
                        onPressed: () {
                          setState(() {
                            _obscureConfirmPassword = !_obscureConfirmPassword;
                          });
                        },
                      ),
                      border: OutlineInputBorder(
                        borderRadius: BorderRadius.circular(12),
                      ),
                      filled: true,
                      fillColor: Colors.white,
                    ),
                    validator: (value) {
                      if (value == null || value.isEmpty) {
                        return 'Please confirm your password';
                      }
                      if (value != passwordController.text) {
                        return 'Passwords do not match';
                      }
                      return null;
                    },
                  ),
                  SizedBox(height: 16),

                  // Address
                  TextFormField(
                    controller: addressController,
                    decoration: InputDecoration(
                      labelText: 'Address',
                      prefixIcon: Icon(Icons.home),
                      border: OutlineInputBorder(
                        borderRadius: BorderRadius.circular(12),
                      ),
                      filled: true,
                      fillColor: Colors.white,
                    ),
                    maxLines: 2,
                    validator: (value) => value == null || value.isEmpty
                        ? 'Please enter your address'
                        : null,
                  ),
                  SizedBox(height: 16),

                  // Aadhar Card Number
                  TextFormField(
                    controller: aadharController,
                    decoration: InputDecoration(
                      labelText: 'Aadhaar Card Number',
                      prefixIcon: Icon(Icons.credit_card),
                      border: OutlineInputBorder(
                        borderRadius: BorderRadius.circular(12),
                      ),
                      filled: true,
                      fillColor: Colors.white,
                    ),
                    keyboardType: TextInputType.number,
                    validator: (value) {
                      if (value == null || value.isEmpty) {
                        return 'Please enter your Aadhaar number';
                      }
                      if (value.length != 12) {
                        return 'Aadhaar number must be 12 digits';
                      }
                      return null;
                    },
                  ),
                  SizedBox(height: 16),

                  // PAN Card Number
                  TextFormField(
                    controller: panController,
                    decoration: InputDecoration(
                      labelText: 'PAN Card Number',
                      prefixIcon: Icon(Icons.card_membership),
                      border: OutlineInputBorder(
                        borderRadius: BorderRadius.circular(12),
                      ),
                      filled: true,
                      fillColor: Colors.white,
                    ),
                    textCapitalization: TextCapitalization.characters,
                    validator: (value) {
                      if (value == null || value.isEmpty) {
                        return 'Please enter your PAN number';
                      }
                      // Basic PAN validation (10 characters)
                      if (value.length != 10) {
                        return 'PAN number must be 10 characters';
                      }
                      return null;
                    },
                  ),
                  SizedBox(height: 24),

                  // Register Button
                  ElevatedButton(
                    onPressed: _isLoading ? null : _registerUser,
                    style: ElevatedButton.styleFrom(
                      backgroundColor: Colors.amber[800],
                      foregroundColor: Colors.white,
                      padding: EdgeInsets.symmetric(vertical: 16),
                      shape: RoundedRectangleBorder(
                        borderRadius: BorderRadius.circular(12),
                      ),
                    ),
                    child: _isLoading
                        ? SizedBox(
                            height: 20,
                            width: 20,
                            child: CircularProgressIndicator(
                              valueColor:
                                  AlwaysStoppedAnimation<Color>(Colors.white),
                              strokeWidth: 2,
                            ),
                          )
                        : Text(
                            'CREATE ACCOUNT',
                            style: TextStyle(
                              fontSize: 16,
                              fontWeight: FontWeight.bold,
                            ),
                          ),
                  ),

                  SizedBox(height: 20),

                  // Back to Login
                  Row(
                    mainAxisAlignment: MainAxisAlignment.center,
                    children: [
                      Text("Already have an account?"),
                      TextButton(
                        onPressed: () {
                          Navigator.pop(context);
                        },
                        child: Text(
                          "Log In",
                          style: TextStyle(
                            color: Colors.amber[800],
                            fontWeight: FontWeight.bold,
                          ),
                        ),
                      ),
                    ],
                  ),
                ],
              ),
            ),
          ),
        ),
      ),
    );
  }

  Future<void> _registerUser() async {
    if (!_formKey.currentState!.validate()) {
      return;
    }

    setState(() => _isLoading = true);

    try {
      // Create user in Firebase Auth
      UserCredential userCredential =
          await FirebaseAuth.instance.createUserWithEmailAndPassword(
        email: emailController.text.trim(),
        password: passwordController.text.trim(),
      );

      // Save user data to Firestore
      await FirebaseFirestore.instance
          .collection('UserData')
          .doc(emailController.text)
          .set({
        'name': nameController.text,
        'number': numberController.text,
        'email': emailController.text,
        'address': addressController.text,
        'aadharCard': aadharController.text,
        'panCard': panController.text,
        'balance': 0,
        'gold': 0,
        'createdAt': FieldValue.serverTimestamp(),
      });

      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Account created successfully!')),
      );

      // Navigate back to login page
      Navigator.pop(context);
    } on FirebaseAuthException catch (e) {
      String errorMessage = 'Registration failed. Please try again.';

      if (e.code == 'weak-password') {
        errorMessage = 'The password provided is too weak.';
      } else if (e.code == 'email-already-in-use') {
        errorMessage = 'This email is already in use.';
      }

      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text(errorMessage)),
      );
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Error: ${e.toString()}')),
      );
    } finally {
      setState(() => _isLoading = false);
    }
  }
}

// Placeholder for your user and admin screens
class LoginPage extends StatefulWidget {
  @override
  _LoginScreenState createState() => _LoginScreenState();
}

class _LoginScreenState extends State<LoginPage>
    with SingleTickerProviderStateMixin {
  final _formKey = GlobalKey<FormState>();

  final TextEditingController emailController = TextEditingController();
  final TextEditingController passwordController = TextEditingController();

  bool _loading = false;
  bool _obscurePassword = true;

  // Animation Controller
  late AnimationController _animationController;
  late Animation<double> _fadeAnimation;

  @override
  void initState() {
    super.initState();
    _animationController = AnimationController(
      vsync: this,
      duration: Duration(milliseconds: 800),
    );
    _fadeAnimation = CurvedAnimation(
      parent: _animationController,
      curve: Curves.easeInOut,
    );
    _animationController.forward();
  }

  @override
  void dispose() {
    _animationController.dispose();
    super.dispose();
  }

  Future<void> _login() async {
    if (!_formKey.currentState!.validate()) return;

    setState(() => _loading = true);

    try {
      // Define admin credentials
      const aeu = "admin123@gmail.com";
      const apu = "admin@123";

      String email = emailController.text.trim();
      String password = passwordController.text.trim();

      // Check if admin credentials were used
      bool isAdmin = (email == aeu && password == apu);

      // Skip Firebase authentication for admin
      if (!isAdmin) {
        await FirebaseAuth.instance.signInWithEmailAndPassword(
          email: email,
          password: password,
        );
      }

      // Set session data
      UserSession.setEmail(email);
      GlobalData.userEmail = email;

      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(
            content: Text(
                isAdmin ? "Admin login successful!" : "Login successful!")),
      );

      // Navigate based on user type
      if (isAdmin) {
        print("Navigating to AdminScreen");
        Navigator.pushReplacement(
            context, MaterialPageRoute(builder: (_) => AdminScreen()));
      } else {
        print("Navigating to UserScreen");
        Navigator.pushReplacement(
            context, MaterialPageRoute(builder: (_) => UserScreen()));
      }
    } on FirebaseAuthException catch (e) {
      String errorMessage = "Login failed. Please try again.";

      if (e.code == 'user-not-found') {
        errorMessage = 'No user found with this email.';
      } else if (e.code == 'wrong-password') {
        errorMessage = 'Incorrect password.';
      }

      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text(errorMessage)),
      );
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text("Error during login: ${e.toString()}")),
      );
    } finally {
      setState(() => _loading = false);
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Container(
        decoration: BoxDecoration(
          image: DecorationImage(
            image: AssetImage('assets/images/1.png'),
            fit: BoxFit.cover,
          ),
        ),
        child: SafeArea(
          child: Center(
            child: SingleChildScrollView(
              child: Padding(
                padding: EdgeInsets.all(24),
                child: FadeTransition(
                  opacity: _fadeAnimation,
                  child: Form(
                    key: _formKey,
                    child: Column(
                      mainAxisAlignment: MainAxisAlignment.center,
                      crossAxisAlignment: CrossAxisAlignment.stretch,
                      children: [
                        // App logo or icon - Wrap with ScaleTransition for extra effect
                        ScaleTransition(
                          scale:
                              _fadeAnimation, // Reuse fade animation for simplicity
                          child: Container(
                            padding: EdgeInsets.all(16),
                            decoration: BoxDecoration(
                              color: Colors.white.withOpacity(0.15),
                              shape: BoxShape.circle,
                              boxShadow: [
                                BoxShadow(
                                  color: Colors.black26,
                                  blurRadius: 15,
                                  spreadRadius: 5,
                                ),
                              ],
                            ),
                            child: Icon(
                              Icons.monetization_on,
                              size: 80,
                              color: Colors.amber[600],
                            ),
                          ),
                        ),
                        SizedBox(height: 24),

                        // App title
                        Text(
                          "Gold Trading App",
                          textAlign: TextAlign.center,
                          style: TextStyle(
                            fontSize: 32,
                            fontWeight: FontWeight.bold,
                            color: Colors.white,
                            shadows: [
                              Shadow(
                                offset: Offset(1, 1),
                                blurRadius: 3.0,
                                color: Colors.black.withOpacity(0.5),
                              ),
                            ],
                          ),
                        ),
                        SizedBox(height: 40),

                        // Email field
                        Container(
                          decoration: BoxDecoration(
                            borderRadius: BorderRadius.circular(12),
                            boxShadow: [
                              BoxShadow(
                                color: Colors.black26,
                                blurRadius: 8,
                                offset: Offset(0, 2),
                              ),
                            ],
                          ),
                          child: TextFormField(
                            controller: emailController,
                            decoration: InputDecoration(
                              labelText: 'Email',
                              labelStyle: TextStyle(color: Colors.amber[100]),
                              prefixIcon:
                                  Icon(Icons.email, color: Colors.amber[100]),
                              border: OutlineInputBorder(
                                borderRadius: BorderRadius.circular(12),
                              ),
                              filled: true,
                              fillColor: Colors.black.withOpacity(0.5),
                              enabledBorder: OutlineInputBorder(
                                borderRadius: BorderRadius.circular(12),
                                borderSide:
                                    BorderSide(color: Colors.amber[600]!),
                              ),
                              focusedBorder: OutlineInputBorder(
                                borderRadius: BorderRadius.circular(12),
                                borderSide: BorderSide(
                                    color: Colors.amber[400]!, width: 2),
                              ),
                            ),
                            style: TextStyle(color: Colors.white),
                            keyboardType: TextInputType.emailAddress,
                            validator: (value) => value == null ||
                                    value.isEmpty ||
                                    !value.contains('@')
                                ? 'Please enter a valid email'
                                : null,
                          ),
                        ),
                        SizedBox(height: 20),

                        // Password field
                        Container(
                          decoration: BoxDecoration(
                            borderRadius: BorderRadius.circular(12),
                            boxShadow: [
                              BoxShadow(
                                color: Colors.black26,
                                blurRadius: 8,
                                offset: Offset(0, 2),
                              ),
                            ],
                          ),
                          child: TextFormField(
                            controller: passwordController,
                            decoration: InputDecoration(
                              labelText: 'Password',
                              labelStyle: TextStyle(color: Colors.amber[100]),
                              prefixIcon:
                                  Icon(Icons.lock, color: Colors.amber[100]),
                              suffixIcon: IconButton(
                                icon: Icon(
                                  _obscurePassword
                                      ? Icons.visibility
                                      : Icons.visibility_off,
                                  color: Colors.amber[100],
                                ),
                                onPressed: () {
                                  setState(() {
                                    _obscurePassword = !_obscurePassword;
                                  });
                                },
                              ),
                              border: OutlineInputBorder(
                                borderRadius: BorderRadius.circular(12),
                              ),
                              filled: true,
                              fillColor: Colors.black.withOpacity(0.5),
                              enabledBorder: OutlineInputBorder(
                                borderRadius: BorderRadius.circular(12),
                                borderSide:
                                    BorderSide(color: Colors.amber[600]!),
                              ),
                              focusedBorder: OutlineInputBorder(
                                borderRadius: BorderRadius.circular(12),
                                borderSide: BorderSide(
                                    color: Colors.amber[400]!, width: 2),
                              ),
                            ),
                            style: TextStyle(color: Colors.white),
                            obscureText: _obscurePassword,
                            validator: (value) => value == null || value.isEmpty
                                ? 'Please enter your password'
                                : null,
                          ),
                        ),

                        // Forgot password link
                        Align(
                          alignment: Alignment.centerRight,
                          child: TextButton(
                            onPressed: () {
                              // TODO: Implement forgot password functionality
                              ScaffoldMessenger.of(context).showSnackBar(
                                SnackBar(
                                    content: Text(
                                        'Forgot password functionality coming soon')),
                              );
                            },
                            child: Text(
                              "Forgot Password?",
                              style: TextStyle(
                                color: Colors.amber[100],
                                fontWeight: FontWeight.bold,
                              ),
                            ),
                          ),
                        ),

                        SizedBox(height: 24),

                        // Login button
                        Container(
                          height: 55,
                          decoration: BoxDecoration(
                            borderRadius: BorderRadius.circular(12),
                            boxShadow: [
                              BoxShadow(
                                color: Colors.black26,
                                blurRadius: 8,
                                offset: Offset(0, 4),
                              ),
                            ],
                            gradient: LinearGradient(
                              colors: [Colors.amber[700]!, Colors.amber[800]!],
                              begin: Alignment.topLeft,
                              end: Alignment.bottomRight,
                            ),
                          ),
                          child: ElevatedButton(
                            onPressed: _loading ? null : _login,
                            child: _loading
                                ? SizedBox(
                                    height: 20,
                                    width: 20,
                                    child: CircularProgressIndicator(
                                      valueColor: AlwaysStoppedAnimation<Color>(
                                          Colors.white),
                                      strokeWidth: 2,
                                    ),
                                  )
                                : Text(
                                    'LOGIN',
                                    style: TextStyle(
                                      fontSize: 16,
                                      fontWeight: FontWeight.bold,
                                    ),
                                  ),
                            style: ElevatedButton.styleFrom(
                              backgroundColor: Colors.transparent,
                              shadowColor: Colors.transparent,
                              foregroundColor: Colors.white,
                              padding: EdgeInsets.symmetric(vertical: 16),
                              shape: RoundedRectangleBorder(
                                borderRadius: BorderRadius.circular(12),
                              ),
                            ),
                          ),
                        ),

                        SizedBox(height: 30),

                        // Create account link
                        Container(
                          padding: EdgeInsets.symmetric(
                              vertical: 12, horizontal: 16),
                          decoration: BoxDecoration(
                            color: Colors.white.withOpacity(0.1),
                            borderRadius: BorderRadius.circular(12),
                          ),
                          child: Row(
                            mainAxisAlignment: MainAxisAlignment.center,
                            children: [
                              Text(
                                "Don't have an account?",
                                style: TextStyle(color: Colors.white),
                              ),
                              TextButton(
                                onPressed: () {
                                  Navigator.push(
                                    context,
                                    MaterialPageRoute(
                                        builder: (context) => UserForm()),
                                  );
                                },
                                child: Text(
                                  "Create One",
                                  style: TextStyle(
                                    color: Colors.amber[300],
                                    fontWeight: FontWeight.bold,
                                  ),
                                ),
                              ),
                            ],
                          ),
                        ),
                      ],
                    ),
                  ),
                ),
              ),
            ),
          ),
        ),
      ),
    );
  }
}

class UserScreen extends StatefulWidget {
  @override
  _UserScreenState createState() => _UserScreenState();
}

class _UserScreenState extends State<UserScreen> {
  double? _goldRate;
  double _balance = 0;
  bool _isLoading = true;
  final PageController _pageController = PageController();
  int _currentPage = 0;
  List<String> _imageUrls = [];
  Timer? _timer;

  @override
  void initState() {
    super.initState();
    _fetchGoldRate();
    _fetchUserBalance();
    _loadCarouselImages();
  }

  Future<void> _fetchGoldRate() async {
    try {
      DocumentSnapshot doc = await FirebaseFirestore.instance
          .collection('goldRate')
          .doc('current')
          .get();

      if (doc.exists && doc['rate'] != null) {
        setState(() {
          _goldRate = doc['rate'].toDouble();
        });
      } else {
        setState(() {
          _goldRate = null;
        });
      }
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Error fetching gold rate')),
      );
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  Future<void> _fetchUserBalance() async {
    if (GlobalData.userEmail == null) return;

    final userDoc = await FirebaseFirestore.instance
        .collection('UserData')
        .doc(GlobalData.userEmail)
        .get();

    setState(() {
      _balance = (userDoc['balance'] ?? 0).toDouble();
    });
  }

  Future<void> _loadCarouselImages() async {
    try {
      final snapshot = await FirebaseFirestore.instance
          .collection('carouselImages')
          .orderBy('createdAt', descending: true)
          .get();

      setState(() {
        _imageUrls = snapshot.docs
            .map((doc) => doc['imageUrl'] as String)
            .where((url) => url.isNotEmpty)
            .toList();
      });

      if (_imageUrls.isNotEmpty) {
        _timer = Timer.periodic(Duration(seconds: 3), (timer) {
          if (_pageController.hasClients) {
            _currentPage = (_currentPage + 1) % _imageUrls.length;
            _pageController.animateToPage(
              _currentPage,
              duration: Duration(milliseconds: 500),
              curve: Curves.easeInOut,
            );
          }
        });
      }
    } catch (e) {
      print('Error loading carousel images: $e');
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Error loading images')),
      );
    }
  }

  @override
  void dispose() {
    _pageController.dispose();
    _timer?.cancel();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      drawer: NavBar(),
      appBar: AppBar(
        title: Text(
          'Gold App Dashboard',
          style: TextStyle(color: Colors.white),
        ),
        backgroundColor: Color(0xFF6A452C),
        elevation: 0,
        actions: [
          Padding(
            padding: const EdgeInsets.only(right: 12),
            child: Container(
              padding: EdgeInsets.symmetric(horizontal: 10, vertical: 6),
              decoration: BoxDecoration(
                color: Colors.amber.shade100,
                borderRadius: BorderRadius.circular(20),
              ),
              child: Row(
                children: [
                  Text(
                    '₹${_balance.toStringAsFixed(2)}',
                    style: TextStyle(fontWeight: FontWeight.bold),
                  ),
                  IconButton(
                    icon: Icon(Icons.add_circle_outline),
                    onPressed: () async {
                      await Navigator.push(
                        context,
                        MaterialPageRoute(builder: (_) => AddMoneyPagen()),
                      );
                      _fetchUserBalance(); // refresh after adding money
                    },
                  ),
                ],
              ),
            ),
          ),
        ],
      ),
      body: Container(
        decoration: BoxDecoration(
          gradient: LinearGradient(
            begin: Alignment.topCenter,
            end: Alignment.bottomCenter,
            colors: [Colors.amber[100]!, Colors.amber[50]!],
          ),
        ),
        child: SingleChildScrollView(
          padding: const EdgeInsets.all(16.0),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Card(
                elevation: 4,
                child: Container(
                  padding: const EdgeInsets.all(16.0),
                  decoration: BoxDecoration(
                    gradient: LinearGradient(
                      begin: Alignment.topLeft,
                      end: Alignment.bottomRight,
                      colors: [Colors.amber[800]!, Colors.amber[600]!],
                    ),
                    borderRadius: BorderRadius.circular(16),
                  ),
                  child: Row(
                    mainAxisAlignment: MainAxisAlignment.spaceAround,
                    children: [
                      Text('Today\'s Gold Rate',
                          style: TextStyle(
                            fontWeight: FontWeight.bold,
                            color: Colors.white,
                          )),
                      _isLoading
                          ? CircularProgressIndicator(
                              valueColor:
                                  AlwaysStoppedAnimation<Color>(Colors.white),
                            )
                          : Text(
                              _goldRate != null ? '₹$_goldRate /g' : 'N/A',
                              style: TextStyle(
                                fontSize: 24,
                                color: Colors.white,
                                fontWeight: FontWeight.bold,
                              ),
                            ),
                    ],
                  ),
                ),
              ),
              SizedBox(height: 20),
              if (_imageUrls.isNotEmpty)
                Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Latest Collection',
                      style: TextStyle(
                        fontSize: 20,
                        fontWeight: FontWeight.bold,
                        color: Colors.amber[800],
                      ),
                    ),
                    SizedBox(height: 12),
                    Container(
                      height: 200,
                      decoration: BoxDecoration(
                        borderRadius: BorderRadius.circular(16),
                        boxShadow: [
                          BoxShadow(
                            color: Colors.black26,
                            offset: Offset(0, 2),
                            blurRadius: 6.0,
                          ),
                        ],
                      ),
                      child: PageView.builder(
                        controller: _pageController,
                        itemCount: _imageUrls.length,
                        itemBuilder: (context, index) {
                          return Padding(
                            padding: const EdgeInsets.symmetric(horizontal: 8),
                            child: ClipRRect(
                              borderRadius: BorderRadius.circular(16),
                              child: Image.network(
                                _imageUrls[index],
                                fit: BoxFit.cover,
                                width: double.infinity,
                                errorBuilder: (context, error, stackTrace) =>
                                    Container(
                                  color: Colors.grey[300],
                                  child: Center(
                                    child: Icon(Icons.broken_image,
                                        size: 50, color: Colors.grey),
                                  ),
                                ),
                              ),
                            ),
                          );
                        },
                      ),
                    ),
                  ],
                ),
              SizedBox(height: 20),
              Row(
                mainAxisAlignment: MainAxisAlignment.spaceBetween,
                crossAxisAlignment: CrossAxisAlignment.center,
                children: [
                  Text(
                    'Products',
                    style: TextStyle(
                      fontSize: 20,
                      fontWeight: FontWeight.bold,
                      color: Colors.amber[800],
                    ),
                  ),
                  GestureDetector(
                    onTap: () {
                      Navigator.push(
                        context,
                        MaterialPageRoute(
                          builder: (context) => ProductListWithFilterPagee(),
                        ),
                      );
                    },
                    child: Text(
                      'View All',
                      style: TextStyle(
                        fontSize: 16,
                        color: Colors.amber[800],
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                  ),
                ],
              ),
              SizedBox(height: 12),
              StreamBuilder<QuerySnapshot>(
                stream: FirebaseFirestore.instance
                    .collection('All')
                    .orderBy('createdAt', descending: true)
                    .limit(10)
                    .snapshots(),
                builder: (context, snapshot) {
                  if (!snapshot.hasData) {
                    return Center(child: CircularProgressIndicator());
                  }
                  final products = snapshot.data!.docs;
                  return Column(
                    children: [
                      ListView.builder(
                        shrinkWrap: true,
                        physics: NeverScrollableScrollPhysics(),
                        itemCount: products.length,
                        itemBuilder: (context, index) {
                          final data =
                              products[index].data() as Map<String, dynamic>;
                          return GestureDetector(
                            onTap: () {
                              Navigator.push(
                                context,
                                MaterialPageRoute(
                                  builder: (_) => ProductDetailsPage(
                                    name: data['name'] ?? 'No Name',
                                    description: data['description'] ?? '',
                                    imageUrl: data['imageUrl'] ?? '',
                                    price: data['price'] ?? 0,
                                  ),
                                ),
                              );
                            },
                            child: Card(
                              shape: RoundedRectangleBorder(
                                borderRadius: BorderRadius.circular(16),
                              ),
                              elevation: 5,
                              margin: const EdgeInsets.symmetric(vertical: 8),
                              child: Row(
                                children: [
                                  ClipRRect(
                                    borderRadius: BorderRadius.only(
                                      topLeft: Radius.circular(16),
                                      bottomLeft: Radius.circular(16),
                                    ),
                                    child: Image.network(
                                      data['imageUrl'] ?? '',
                                      width: 100,
                                      height: 100,
                                      fit: BoxFit.cover,
                                      errorBuilder:
                                          (context, error, stackTrace) =>
                                              Container(
                                        width: 100,
                                        height: 100,
                                        color: Colors.grey[300],
                                        child: Icon(Icons.image_not_supported,
                                            color: Colors.grey),
                                      ),
                                    ),
                                  ),
                                  SizedBox(width: 12),
                                  Expanded(
                                    child: Padding(
                                      padding: const EdgeInsets.symmetric(
                                          vertical: 10, horizontal: 5),
                                      child: Column(
                                        crossAxisAlignment:
                                            CrossAxisAlignment.start,
                                        children: [
                                          Text(
                                            data['name'] ?? 'No Name',
                                            style: TextStyle(
                                                fontWeight: FontWeight.bold,
                                                fontSize: 18),
                                          ),
                                          SizedBox(height: 4),
                                          Text(
                                            data['description'] ?? '',
                                            maxLines: 2,
                                            overflow: TextOverflow.ellipsis,
                                            style: TextStyle(
                                                color: Colors.grey[700]),
                                          ),
                                          SizedBox(height: 8),
                                          Text(
                                            '₹${data['price'] ?? '--'}',
                                            style: TextStyle(
                                              fontWeight: FontWeight.bold,
                                              color: Colors.amber[800],
                                              fontSize: 16,
                                            ),
                                          ),
                                        ],
                                      ),
                                    ),
                                  ),
                                ],
                              ),
                            ),
                          );
                        },
                      ),
                    ],
                  );
                },
              ),
            ],
          ),
        ),
      ),
    );
  }
}

class ProductDetailsPage extends StatelessWidget {
  final String name;
  final String description;
  final String imageUrl;
  final dynamic price;

  ProductDetailsPage({
    required this.name,
    required this.description,
    required this.imageUrl,
    required this.price,
  });

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(
          name,
          style: TextStyle(color: Colors.white),
        ),
        backgroundColor: Color(0xFF6A452C),
        foregroundColor: Colors.white,
        // Consistent with your UserScreen
        elevation: 0,
      ),
      body: Container(
        decoration: BoxDecoration(
          gradient: LinearGradient(
            begin: Alignment.topCenter,
            end: Alignment.bottomCenter,
            colors: [Colors.amber[100]!, Colors.amber[50]!],
          ),
        ),
        child: SingleChildScrollView(
          padding: const EdgeInsets.all(16),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              // Product Image
              Card(
                elevation: 5,
                shape: RoundedRectangleBorder(
                  borderRadius: BorderRadius.circular(16),
                ),
                child: ClipRRect(
                  borderRadius: BorderRadius.circular(16),
                  child: Image.network(
                    imageUrl,
                    height: 300,
                    width: double.infinity,
                    fit: BoxFit.cover,
                    loadingBuilder: (context, child, loadingProgress) {
                      if (loadingProgress == null) return child;
                      return Container(
                        height: 300,
                        color: Colors.grey[200],
                        child: Center(
                          child: CircularProgressIndicator(),
                        ),
                      );
                    },
                    errorBuilder: (context, error, stackTrace) => Container(
                      height: 300,
                      color: Colors.grey[300],
                      child: Center(
                        child: Icon(Icons.image_not_supported,
                            size: 50, color: Colors.grey),
                      ),
                    ),
                  ),
                ),
              ),
              SizedBox(height: 24),

              // Product Name
              Text(
                name,
                style: TextStyle(
                  fontSize: 28,
                  fontWeight: FontWeight.bold,
                  color: Colors.amber[800],
                ),
              ),
              SizedBox(height: 16),

              // Price
              Container(
                padding: EdgeInsets.symmetric(vertical: 8, horizontal: 16),
                decoration: BoxDecoration(
                  color: Colors.amber[50],
                  borderRadius: BorderRadius.circular(8),
                  border: Border.all(color: Colors.amber[800]!),
                ),
                child: Text(
                  '₹${price is double ? price.toStringAsFixed(2) : price}',
                  style: TextStyle(
                    fontSize: 24,
                    fontWeight: FontWeight.bold,
                    color: Colors.amber[800],
                  ),
                ),
              ),
              SizedBox(height: 24),

              // Description
              Text(
                'Description',
                style: TextStyle(
                  fontSize: 20,
                  fontWeight: FontWeight.bold,
                  color: Colors.amber[800],
                ),
              ),
              SizedBox(height: 8),
              Text(
                description,
                style: TextStyle(
                  fontSize: 16,
                  color: Colors.grey[800],
                  height: 1.5,
                ),
              ),
              SizedBox(height: 24),

              // Buy Button
              SizedBox(
                width: double.infinity,
                child: ElevatedButton(
                  onPressed: () {
                    // Add your buy logic here
                  },
                  style: ElevatedButton.styleFrom(
                    backgroundColor: Colors.amber[800],
                    foregroundColor: Colors.white,
                    padding: EdgeInsets.symmetric(vertical: 16),
                    shape: RoundedRectangleBorder(
                      borderRadius: BorderRadius.circular(12),
                    ),
                  ),
                  child: Text(
                    'BUY NOW',
                    style: TextStyle(
                      fontSize: 18,
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

class NavBar extends StatefulWidget {
  const NavBar({Key? key}) : super(key: key);

  @override
  _NavBarState createState() => _NavBarState();
}

class _NavBarState extends State<NavBar> with TickerProviderStateMixin {
  String userEmail = 'Loading...';
  late AnimationController _tapAnimationController;
  late AnimationController _listAnimationController;
  late List<Animation<Offset>> _slideAnimations;
  int _selectedIndex = -1;
  final List<IconData> _icons = [
    Icons.gif_outlined,
    Icons.calculate,
    Icons.currency_exchange,
    Icons.person,
    Icons.description,
    Icons.logout,
  ];

  @override
  void initState() {
    super.initState();
    fetchUserEmail();

    // Controller for tap animation
    _tapAnimationController = AnimationController(
      vsync: this,
      duration: Duration(milliseconds: 150),
    );

    // Controller for list animation
    _listAnimationController = AnimationController(
      vsync: this,
      duration: Duration(milliseconds: 500),
    );

    // Create staggered animations for list items
    _slideAnimations = List.generate(
      _icons.length,
      (index) => Tween<Offset>(
        begin: Offset(-1.0, 0.0),
        end: Offset.zero,
      ).animate(CurvedAnimation(
        parent: _listAnimationController,
        curve: Interval(
          index * (1.0 / _icons.length) * 0.6,
          (index + 1) * (1.0 / _icons.length),
          curve: Curves.easeOutCubic,
        ),
      )),
    );

    _listAnimationController.forward();
  }

  @override
  void dispose() {
    _tapAnimationController.dispose();
    _listAnimationController.dispose();
    super.dispose();
  }

  void fetchUserEmail() {
    String? email = UserSession.getEmail();
    setState(() {
      userEmail = email ?? 'No email found';
    });
  }

  Widget _buildAnimatedListTile({
    required int index,
    required IconData icon,
    required String title,
    required VoidCallback onTap,
    required Animation<Offset> slideAnimation,
  }) {
    return SlideTransition(
      position: slideAnimation,
      child: MouseRegion(
        onEnter: (_) => setState(() => _selectedIndex = index),
        onExit: (_) => setState(() => _selectedIndex = -1),
        child: AnimatedContainer(
          duration: Duration(milliseconds: 200),
          decoration: BoxDecoration(
            gradient: _selectedIndex == index
                ? LinearGradient(
                    colors: [Colors.amber.shade300, Colors.amber.shade100],
                    begin: Alignment.centerLeft,
                    end: Alignment.centerRight,
                  )
                : null,
            borderRadius: BorderRadius.circular(8.0),
          ),
          margin: EdgeInsets.symmetric(horizontal: 8, vertical: 2),
          child: ListTile(
            leading: AnimatedContainer(
              duration: Duration(milliseconds: 200),
              padding: EdgeInsets.all(4),
              decoration: BoxDecoration(
                color: _selectedIndex == index
                    ? Colors.amber[800]
                    : Colors.transparent,
                borderRadius: BorderRadius.circular(8),
              ),
              child: Icon(
                icon,
                color:
                    _selectedIndex == index ? Colors.white : Colors.amber[800],
              ),
            ),
            title: Text(
              title,
              style: TextStyle(
                fontWeight: _selectedIndex == index
                    ? FontWeight.bold
                    : FontWeight.normal,
                color: _selectedIndex == index
                    ? Colors.amber[800]
                    : Colors.black87,
              ),
            ),
            onTap: () {
              _tapAnimationController.forward().then((_) {
                _tapAnimationController.reverse();
                onTap();
              });
            },
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(8),
            ),
          ),
        ),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    // Navigation destinations with their actions
    final navItems = [
      {
        'title': "All Products",
        'action': () {
          Navigator.pop(context);
          Navigator.push(
            context,
            MaterialPageRoute(
                builder: (context) => ProductListWithFilterPagee()),
          );
        },
      },
      {
        'title': "Gold Calculator",
        'action': () {
          Navigator.pop(context);
          Navigator.push(
            context,
            MaterialPageRoute(builder: (context) => GoldRateCalculator()),
          );
        },
      },
      {
        'title': "Buy/Sell Gold",
        'action': () {
          Navigator.pop(context);
          Navigator.push(
            context,
            MaterialPageRoute(builder: (context) => BuySellGoldPage()),
          );
        },
      },
      {
        'title': "Profile",
        'action': () {
          Navigator.pop(context);
          Navigator.push(
            context,
            MaterialPageRoute(builder: (context) => ProfileScreen()),
          );
        },
      },
      {
        'title': "Terms & Conditions",
        'action': () {
          Navigator.pop(context);
          Navigator.push(
            context,
            MaterialPageRoute(
                builder: (context) => UserTermsAndConditionsPage()),
          );
        },
      },
      {
        'title': "Logout",
        'action': () {
          Navigator.pop(context);
          UserSession.clear();
          Navigator.pushAndRemoveUntil(
            context,
            MaterialPageRoute(builder: (context) => LoginPage()),
            (Route<dynamic> route) => false,
          );
        },
      },
    ];

    return Drawer(
      elevation: 16.0,
      backgroundColor: Colors.amber[50],
      child: Column(
        children: [
          // Header with user info
          Container(
            decoration: BoxDecoration(
              gradient: LinearGradient(
                colors: [Colors.amber[800]!, Colors.amber[600]!],
                begin: Alignment.topLeft,
                end: Alignment.bottomRight,
              ),
              boxShadow: [
                BoxShadow(
                  color: Colors.amber.withOpacity(0.5),
                  spreadRadius: 1,
                  blurRadius: 4,
                  offset: Offset(0, 2),
                ),
              ],
            ),
            child: UserAccountsDrawerHeader(
              decoration: BoxDecoration(
                color: Colors.transparent,
              ),
              accountName: Text("Welcome 👋",
                  style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold)),
              accountEmail: Text(userEmail),
              currentAccountPicture: CircleAvatar(
                backgroundColor: Colors.white,
                child: Icon(Icons.person, color: Colors.amber[800], size: 40),
              ),
            ),
          ),
          SizedBox(height: 8),

          // Navigation items
          Expanded(
            child: ListView.builder(
              padding: EdgeInsets.symmetric(vertical: 8),
              itemCount: _icons.length,
              itemBuilder: (context, index) {
                // Add divider before Logout
                if (index == _icons.length - 1) {
                  return Column(
                    children: [
                      Divider(thickness: 1, color: Colors.amber[200]),
                      _buildAnimatedListTile(
                        index: index,
                        icon: _icons[index],
                        title: navItems[index]['title'] as String,
                        onTap: navItems[index]['action'] as VoidCallback,
                        slideAnimation: _slideAnimations[index],
                      ),
                    ],
                  );
                }

                return _buildAnimatedListTile(
                  index: index,
                  icon: _icons[index],
                  title: navItems[index]['title'] as String,
                  onTap: navItems[index]['action'] as VoidCallback,
                  slideAnimation: _slideAnimations[index],
                );
              },
            ),
          ),

          // Footer with app version
          Container(
            padding: EdgeInsets.symmetric(vertical: 12, horizontal: 16),
            decoration: BoxDecoration(
              color: Colors.amber[100],
              boxShadow: [
                BoxShadow(
                  color: Colors.black.withOpacity(0.05),
                  spreadRadius: 1,
                  blurRadius: 4,
                  offset: Offset(0, -2),
                ),
              ],
            ),
            child: Row(
              children: [
                Icon(Icons.verified, color: Colors.amber[800]),
                SizedBox(width: 8),
                Text(
                  'Gold Trading App v1.0',
                  style: TextStyle(
                    color: Colors.amber[800],
                    fontWeight: FontWeight.bold,
                  ),
                ),
              ],
            ),
          ),
        ],
      ),
    );
  }
}

class UserTermsAndConditionsPage extends StatefulWidget {
  @override
  _UserTermsAndConditionsPageState createState() =>
      _UserTermsAndConditionsPageState();
}

class _UserTermsAndConditionsPageState
    extends State<UserTermsAndConditionsPage> {
  List<String> _terms = [];
  bool _isLoading = true;

  @override
  void initState() {
    super.initState();
    _fetchTerms();
  }

  void _fetchTerms() async {
    try {
      DocumentSnapshot doc = await FirebaseFirestore.instance
          .collection('appSettings')
          .doc('terms')
          .get();
      if (doc.exists && doc['content'] != null) {
        setState(() {
          _terms = List<String>.from(doc['content']);
        });
      }
    } catch (e) {
      print("Error fetching terms: $e");
    } finally {
      setState(() => _isLoading = false);
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(
          "Terms & Conditions",
          style: TextStyle(color: Colors.white),
        ),
        backgroundColor: Color(0xFF6A452C),
        foregroundColor: Colors.white,
        // Consistent with your app theme
        elevation: 0,
      ),
      body: Container(
        decoration: BoxDecoration(
          gradient: LinearGradient(
            begin: Alignment.topCenter,
            end: Alignment.bottomCenter,
            colors: [Colors.amber[100]!, Colors.amber[50]!],
          ),
        ),
        child: _isLoading
            ? Center(
                child: CircularProgressIndicator(
                  valueColor: AlwaysStoppedAnimation<Color>(Colors.amber[800]!),
                ),
              )
            : Padding(
                padding: const EdgeInsets.all(16.0),
                child: Card(
                  elevation: 5,
                  shape: RoundedRectangleBorder(
                    borderRadius: BorderRadius.circular(16),
                  ),
                  child: Padding(
                    padding: const EdgeInsets.all(16.0),
                    child: ListView.builder(
                      itemCount: _terms.length,
                      itemBuilder: (context, index) {
                        return Padding(
                          padding: const EdgeInsets.symmetric(vertical: 8.0),
                          child: Column(
                            crossAxisAlignment: CrossAxisAlignment.start,
                            children: [
                              Text(
                                "${_terms[index]}",
                                style: TextStyle(
                                  fontSize: 16,
                                  color: Colors.grey[800],
                                  height: 1.5,
                                ),
                              ),
                              if (index < _terms.length - 1)
                                Divider(
                                  color: Colors.amber[200],
                                  thickness: 1,
                                ),
                            ],
                          ),
                        );
                      },
                    ),
                  ),
                ),
              ),
      ),
    );
  }
}

class BuySellGoldPage extends StatefulWidget {
  @override
  _BuySellGoldPageState createState() => _BuySellGoldPageState();
}

class _BuySellGoldPageState extends State<BuySellGoldPage> {
  double? _goldRate;
  double _balance = 0;
  double _userGold = 0;
  double _calculatedTotal = 0;
  bool _isLoading = true;

  final TextEditingController _gramController = TextEditingController();

  @override
  void initState() {
    super.initState();
    _fetchGoldRate();
    _fetchUserData();
    _gramController.addListener(_calculateTotal);
  }

  @override
  void dispose() {
    _gramController.removeListener(_calculateTotal);
    _gramController.dispose();
    super.dispose();
  }

  void _calculateTotal() {
    final text = _gramController.text.trim();
    final grams = double.tryParse(text);
    if (_goldRate != null && grams != null) {
      setState(() {
        _calculatedTotal = grams * _goldRate!;
      });
    } else {
      setState(() {
        _calculatedTotal = 0;
      });
    }
  }

  Future<void> _fetchGoldRate() async {
    DocumentSnapshot doc = await FirebaseFirestore.instance
        .collection('goldRate')
        .doc('current')
        .get();

    if (doc.exists && doc['rate'] != null) {
      setState(() {
        _goldRate = doc['rate'].toDouble();
      });
    }
  }

  Future<void> _fetchUserData() async {
    if (GlobalData.userEmail == null) return;

    DocumentSnapshot userDoc = await FirebaseFirestore.instance
        .collection('UserData')
        .doc(GlobalData.userEmail)
        .get();

    setState(() {
      _balance = (userDoc['balance'] ?? 0).toDouble();
      _userGold = (userDoc['gold'] ?? 0).toDouble();
      _isLoading = false;
    });
  }

  Future<void> _updateUser(double newGold, double newBalance) async {
    await FirebaseFirestore.instance
        .collection('UserData')
        .doc(GlobalData.userEmail)
        .update({
      'gold': newGold,
      'balance': newBalance,
    });
    _fetchUserData(); // Refresh UI
  }

  void _handleTransaction(String type) {
    final gramText = _gramController.text.trim();
    if (gramText.isEmpty ||
        double.tryParse(gramText) == null ||
        _goldRate == null) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Enter valid grams')),
      );
      return;
    }

    final grams = double.parse(gramText);
    final total = grams * _goldRate!;

    if (type == 'Buy' && total > _balance) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Insufficient balance')),
      );
      return;
    }

    if (type == 'Sell' && grams > _userGold) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Not enough gold')),
      );
      return;
    }

    showDialog(
      context: context,
      builder: (_) => AlertDialog(
        title: Text('$type Gold'),
        content: Text(
          'Grams: $grams\nRate: ₹$_goldRate\nTotal: ₹${total.toStringAsFixed(2)}',
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(context),
            child: Text('Cancel'),
          ),
          ElevatedButton(
            onPressed: () async {
              Navigator.pop(context);

              double newGold =
                  type == 'Buy' ? _userGold + grams : _userGold - grams;
              double newBalance =
                  type == 'Buy' ? _balance - total : _balance + total;

              await _updateUser(newGold, newBalance);

              // Save transaction to Firestore
              await FirebaseFirestore.instance
                  .collection('GoldTransactions')
                  .add({
                'email': GlobalData.userEmail,
                'type': type,
                'grams': grams,
                'rate': _goldRate,
                'total': total,
                'timestamp': Timestamp.now(),
              });

              _gramController.clear();

              ScaffoldMessenger.of(context).showSnackBar(
                SnackBar(content: Text('$type successful and saved 🧾✨')),
              );
            },
            child: Text('Confirm $type'),
          ),
        ],
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Buy / Sell Gold", style: TextStyle(color: Colors.white)),
        foregroundColor: Colors.white,
        backgroundColor: Color(0xFF6A452C),
        actions: [
          Padding(
            padding: const EdgeInsets.only(right: 12),
            child: Row(
              children: [
                Container(
                  padding: EdgeInsets.symmetric(horizontal: 10, vertical: 6),
                  decoration: BoxDecoration(
                    borderRadius: BorderRadius.circular(20),
                  ),
                  child: Row(
                    children: [
                      Text(
                        '₹${_balance.toStringAsFixed(2)}',
                        style: TextStyle(fontWeight: FontWeight.bold),
                      ),
                      IconButton(
                        icon: Icon(Icons.add_circle_outline),
                        onPressed: () {
                          Navigator.push(
                            context,
                            MaterialPageRoute(builder: (_) => AddMoneyPage()),
                          ).then((_) => _fetchUserData());
                        },
                      ),
                    ],
                  ),
                ),
              ],
            ),
          ),
        ],
      ),
      backgroundColor: Colors.amber[100],
      body: _isLoading
          ? Center(child: CircularProgressIndicator())
          : Padding(
              padding: const EdgeInsets.all(16),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.stretch,
                children: [
                  Card(
                    elevation: 4,
                    child: ListTile(
                      title: Text('Current Gold Rate'),
                      trailing: Text(
                        _goldRate != null ? '₹$_goldRate/g' : 'N/A',
                        style: TextStyle(fontSize: 18, color: Colors.amber),
                      ),
                    ),
                  ),
                  SizedBox(height: 16),
                  Text('Your Gold: $_userGold g',
                      style:
                          TextStyle(fontSize: 16, fontWeight: FontWeight.w500)),
                  SizedBox(height: 10),
                  TextField(
                    controller: _gramController,
                    keyboardType:
                        TextInputType.numberWithOptions(decimal: true),
                    decoration: InputDecoration(
                      labelText: 'Enter Grams',
                      border: OutlineInputBorder(),
                      prefixIcon: Icon(Icons.balance),
                    ),
                  ),
                  SizedBox(height: 10),
                  Text(
                    'Total: ₹${_calculatedTotal.toStringAsFixed(2)}',
                    style: TextStyle(fontSize: 16, color: Colors.grey[700]),
                  ),
                  SizedBox(height: 20),
                  Row(
                    mainAxisAlignment: MainAxisAlignment.spaceAround,
                    children: [
                      ElevatedButton.icon(
                        onPressed: () => _handleTransaction('Buy'),
                        icon: Icon(Icons.shopping_cart_outlined),
                        label: Text('Buy'),
                      ),
                      ElevatedButton.icon(
                        onPressed: () => _handleTransaction('Sell'),
                        style: ElevatedButton.styleFrom(
                            backgroundColor: Colors.redAccent),
                        icon: Icon(Icons.sell),
                        label: Text('Sell'),
                      ),
                    ],
                  ),
                ],
              ),
            ),
    );
  }
}

class AddMoneyPage extends StatefulWidget {
  @override
  _AddMoneyPageState createState() => _AddMoneyPageState();
}

class _AddMoneyPageState extends State<AddMoneyPage> {
  final TextEditingController _moneyController = TextEditingController();
  double _currentBalance = 0;
  bool _isLoading = true;

  @override
  void initState() {
    super.initState();
    _fetchUserBalance();
  }

  Future<void> _fetchUserBalance() async {
    try {
      if (GlobalData.userEmail == null) throw 'User email is null';

      DocumentSnapshot doc = await FirebaseFirestore.instance
          .collection('UserData')
          .doc(GlobalData.userEmail)
          .get();

      if (doc.exists && doc.data() != null) {
        setState(() {
          _currentBalance = doc['balance'].toDouble();
        });
      }
    } catch (e) {
      print('Error fetching balance: $e');
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Failed to fetch balance')),
      );
    } finally {
      setState(() => _isLoading = false);
    }
  }

  void _goToGooglePay() {
    String text = _moneyController.text.trim();
    if (text.isEmpty ||
        double.tryParse(text) == null ||
        double.parse(text) <= 0) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Enter a valid amount')),
      );
      return;
    }

    double amount = double.parse(text);
    Navigator.push(
      context,
      MaterialPageRoute(
        builder: (context) => GooglePayPage(amount: amount),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.grey[100],
      appBar: AppBar(
        title: Text("Add Money", style: TextStyle(color: Colors.white)),
        foregroundColor: Colors.white,
        backgroundColor: Color(0xFF6A452C),
        elevation: 0,
      ),
      body: _isLoading
          ? Center(child: CircularProgressIndicator())
          : Padding(
              padding: const EdgeInsets.all(20.0),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.stretch,
                children: [
                  Container(
                    padding: EdgeInsets.all(25),
                    decoration: BoxDecoration(
                      color: Colors.amber[100],
                      borderRadius: BorderRadius.circular(16),
                      boxShadow: [
                        BoxShadow(
                          color: Colors.black12,
                          blurRadius: 10,
                          offset: Offset(0, 4),
                        ),
                      ],
                    ),
                    child: Column(
                      children: [
                        Text("Your Current Balance",
                            style: TextStyle(
                                fontSize: 16,
                                color: Colors.grey[800],
                                fontWeight: FontWeight.w500)),
                        SizedBox(height: 10),
                        Text(
                          '₹${_currentBalance.toStringAsFixed(2)}',
                          style: TextStyle(
                            fontSize: 28,
                            fontWeight: FontWeight.bold,
                            color: Colors.black87,
                          ),
                        ),
                      ],
                    ),
                  ),
                  SizedBox(height: 30),
                  TextField(
                    controller: _moneyController,
                    keyboardType:
                        TextInputType.numberWithOptions(decimal: true),
                    style: TextStyle(fontSize: 18),
                    decoration: InputDecoration(
                      labelText: 'Enter Amount',
                      prefixIcon: Icon(Icons.currency_rupee_rounded),
                      border: OutlineInputBorder(
                        borderRadius: BorderRadius.circular(12),
                      ),
                      filled: true,
                      fillColor: Colors.white,
                    ),
                  ),
                  SizedBox(height: 30),
                  ElevatedButton.icon(
                    onPressed: _goToGooglePay,
                    icon: Icon(Icons.add_circle_outline),
                    label: Text(
                      "Add Money",
                      style: TextStyle(fontSize: 18),
                    ),
                    style: ElevatedButton.styleFrom(
                      backgroundColor: Colors.amber[800],
                      padding: EdgeInsets.symmetric(vertical: 14),
                      shape: RoundedRectangleBorder(
                        borderRadius: BorderRadius.circular(12),
                      ),
                      elevation: 5,
                    ),
                  ),
                ],
              ),
            ),
    );
  }
}

class AddMoneyPagen extends StatefulWidget {
  @override
  _AddMoneyPageStaten createState() => _AddMoneyPageStaten();
}

class _AddMoneyPageStaten extends State<AddMoneyPagen> {
  double _currentBalance = 0;
  bool _isLoading = true;

  @override
  void initState() {
    super.initState();
    _fetchUserBalance();
  }

  Future<void> _fetchUserBalance() async {
    try {
      if (GlobalData.userEmail == null) throw 'User email is null';

      DocumentSnapshot doc = await FirebaseFirestore.instance
          .collection('UserData')
          .doc(GlobalData.userEmail)
          .get();

      if (doc.exists && doc.data() != null) {
        setState(() {
          _currentBalance = doc['balance'].toDouble();
        });
      }
    } catch (e) {
      print('Error fetching balance: $e');
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Failed to fetch balance')),
      );
    } finally {
      setState(() => _isLoading = false);
    }
  }

  @override
  Widget build(BuildContext context) {
    final accentColor = Color(0xFFD4AF37); // Gold color

    return Scaffold(
      appBar: AppBar(
        title: Text("Wallet Options "),
        backgroundColor: Color(0xFF6A452C),
        foregroundColor: Colors.white,
      ),
      body: _isLoading
          ? Center(child: CircularProgressIndicator())
          : Container(
              width: double.infinity,
              decoration: BoxDecoration(
                gradient: LinearGradient(
                  colors: [Colors.amber.shade100, Colors.amber.shade50],
                  begin: Alignment.topCenter,
                  end: Alignment.bottomCenter,
                ),
              ),
              child: Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  Card(
                    elevation: 6,
                    shape: RoundedRectangleBorder(
                      borderRadius: BorderRadius.circular(16),
                    ),
                    margin: EdgeInsets.symmetric(horizontal: 24),
                    child: Padding(
                      padding: const EdgeInsets.all(24.0),
                      child: Column(
                        children: [
                          Text(
                            "Current Balance",
                            style: TextStyle(
                              fontSize: 22,
                              fontWeight: FontWeight.w600,
                              color: Colors.grey[800],
                            ),
                          ),
                          SizedBox(height: 8),
                          Text(
                            "₹$_currentBalance",
                            style: TextStyle(
                              fontSize: 32,
                              fontWeight: FontWeight.bold,
                              color: accentColor,
                            ),
                          ),
                        ],
                      ),
                    ),
                  ),
                  SizedBox(height: 40),
                  ElevatedButton.icon(
                    icon: Icon(Icons.add_circle_outline),
                    style: ElevatedButton.styleFrom(
                      backgroundColor: accentColor,
                      padding:
                          EdgeInsets.symmetric(horizontal: 24, vertical: 14),
                      shape: RoundedRectangleBorder(
                        borderRadius: BorderRadius.circular(30),
                      ),
                      textStyle: TextStyle(fontSize: 16),
                    ),
                    label: Text("➕ Add Money"),
                    onPressed: () {
                      Navigator.push(
                        context,
                        MaterialPageRoute(builder: (_) => AddMoneyPage()),
                      );
                    },
                  ),
                  SizedBox(height: 20),
                  ElevatedButton.icon(
                    icon: Icon(Icons.money_off_csred),
                    style: ElevatedButton.styleFrom(
                      backgroundColor: Colors.amber[100],
                      padding:
                          EdgeInsets.symmetric(horizontal: 24, vertical: 14),
                      shape: RoundedRectangleBorder(
                        borderRadius: BorderRadius.circular(30),
                      ),
                      textStyle: TextStyle(fontSize: 16),
                    ),
                    label: Text("💸 Withdraw Money"),
                    onPressed: () {
                      Navigator.push(
                        context,
                        MaterialPageRoute(
                          builder: (_) => WithdrawMoneyPage(
                              currentBalance: _currentBalance),
                        ),
                      );
                    },
                  ),
                ],
              ),
            ),
    );
  }
}

class WithdrawMoneyPage extends StatefulWidget {
  final double currentBalance;

  WithdrawMoneyPage({required this.currentBalance});

  @override
  _WithdrawMoneyPageState createState() => _WithdrawMoneyPageState();
}

class _WithdrawMoneyPageState extends State<WithdrawMoneyPage> {
  final _formKey = GlobalKey<FormState>();
  final TextEditingController _nameController = TextEditingController();
  final TextEditingController _bankController = TextEditingController();
  final TextEditingController _accountController = TextEditingController();
  final TextEditingController _ifscController = TextEditingController();
  final TextEditingController _amountController = TextEditingController();

  bool _isSubmitting = false;
  double _balance = 0;

  @override
  void initState() {
    super.initState();
    _balance = widget.currentBalance;
  }

  Future<void> _submitWithdrawal() async {
    if (!_formKey.currentState!.validate()) return;

    double amount = double.parse(_amountController.text.trim());

    if (amount > _balance) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Insufficient balance')),
      );
      return;
    }

    setState(() => _isSubmitting = true);

    try {
      // 1. Submit withdrawal request
      await FirebaseFirestore.instance.collection('WithdrawRequests').add({
        'email': GlobalData.userEmail,
        'name': _nameController.text.trim(),
        'bank': _bankController.text.trim(),
        'accountNumber': _accountController.text.trim(),
        'ifsc': _ifscController.text.trim(),
        'amount': amount,
        'timestamp': Timestamp.now(),
      });

      // 2. Update balance in UserData
      double updatedBalance = _balance - amount;

      await FirebaseFirestore.instance
          .collection('UserData')
          .doc(GlobalData.userEmail)
          .update({'balance': updatedBalance});

      setState(() {
        _balance = updatedBalance;
      });

      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Withdrawal request submitted 🚀')),
      );

      Navigator.pop(context);
    } catch (e) {
      print("Error: $e");
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Something went wrong ')),
      );
    } finally {
      setState(() => _isSubmitting = false);
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.grey[100],
      appBar: AppBar(
        title: Text("Withdraw to Bank", style: TextStyle(color: Colors.white)),
        foregroundColor: Colors.white,
        backgroundColor: Color(0xFF6A452C),
        elevation: 0,
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Form(
          key: _formKey,
          child: ListView(
            children: [
              Container(
                padding: EdgeInsets.all(20),
                decoration: BoxDecoration(
                  color: Colors.red[50],
                  borderRadius: BorderRadius.circular(12),
                  boxShadow: [
                    BoxShadow(
                      color: Colors.black12,
                      blurRadius: 6,
                      offset: Offset(0, 3),
                    ),
                  ],
                ),
                child: Column(
                  children: [
                    Text("Available Balance",
                        style: TextStyle(
                            fontSize: 16,
                            color: Colors.black54,
                            fontWeight: FontWeight.w500)),
                    SizedBox(height: 6),
                    Text(
                      "₹${_balance.toStringAsFixed(2)}",
                      style: TextStyle(
                        fontSize: 26,
                        fontWeight: FontWeight.bold,
                        color: Colors.black87,
                      ),
                    ),
                  ],
                ),
              ),
              SizedBox(height: 25),
              _buildInputField(
                controller: _nameController,
                label: "Account Holder Name",
                icon: Icons.person,
              ),
              _buildInputField(
                controller: _bankController,
                label: "Bank Name",
                icon: Icons.account_balance,
              ),
              _buildInputField(
                controller: _accountController,
                label: "Account Number",
                icon: Icons.numbers,
                isNumber: true,
              ),
              _buildInputField(
                controller: _ifscController,
                label: "IFSC Code",
                icon: Icons.code,
              ),
              _buildInputField(
                controller: _amountController,
                label: "Withdraw Amount",
                icon: Icons.currency_rupee,
                isNumber: true,
                validator: (val) {
                  if (val == null || val.isEmpty) return 'Required';
                  final num = double.tryParse(val);
                  if (num == null || num <= 0) return 'Enter valid amount';
                  if (num > _balance) return 'Amount exceeds balance';
                  return null;
                },
              ),
              SizedBox(height: 20),
              _isSubmitting
                  ? Center(child: CircularProgressIndicator())
                  : ElevatedButton.icon(
                      onPressed: _submitWithdrawal,
                      icon: Icon(Icons.send),
                      label: Text(
                        "Submit Withdrawal",
                        style: TextStyle(fontSize: 18),
                      ),
                      style: ElevatedButton.styleFrom(
                        backgroundColor: Colors.amber[800],
                        padding: EdgeInsets.symmetric(vertical: 14),
                        shape: RoundedRectangleBorder(
                          borderRadius: BorderRadius.circular(12),
                        ),
                      ),
                    )
            ],
          ),
        ),
      ),
    );
  }

  Widget _buildInputField({
    required TextEditingController controller,
    required String label,
    required IconData icon,
    bool isNumber = false,
    String? Function(String?)? validator,
  }) {
    return Padding(
      padding: const EdgeInsets.symmetric(vertical: 10),
      child: TextFormField(
        controller: controller,
        keyboardType:
            isNumber ? TextInputType.numberWithOptions(decimal: true) : null,
        validator: validator ?? (val) => val!.isEmpty ? 'Required' : null,
        decoration: InputDecoration(
          labelText: label,
          prefixIcon: Icon(icon),
          filled: true,
          fillColor: Colors.white,
          border: OutlineInputBorder(borderRadius: BorderRadius.circular(12)),
        ),
      ),
    );
  }
}

class GooglePayPage extends StatefulWidget {
  final double amount;

  GooglePayPage({required this.amount});

  @override
  _GooglePayPageState createState() => _GooglePayPageState();
}

class _GooglePayPageState extends State<GooglePayPage> {
  UpiIndia _upiIndia = UpiIndia();
  UpiApp? _gpay;

  @override
  void initState() {
    super.initState();
    // Check if the platform is Android or iOS
    if (Foundation.kIsWeb) {
      // Web doesn't support UPI payment, so show an alternate message or UI.
      setState(() {
        _gpay = null;
      });
    } else {
      _getUpiApps();
    }
  }

  Future<void> _getUpiApps() async {
    try {
      List<UpiApp> apps =
          await _upiIndia.getAllUpiApps(mandatoryTransactionId: false);
      setState(() {
        _gpay = apps.firstWhere(
          (app) => app.name.toLowerCase().contains("google"),
          orElse: () => apps.first,
        );
      });
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text("Failed to load UPI apps")),
      );
    }
  }

  Future<void> _initiateTransaction() async {
    if (_gpay == null) return;

    UpiResponse response = await _upiIndia.startTransaction(
      app: _gpay!,
      receiverUpiId:
          "dharmiksinhvaja15@okhdfcbank", // 🔁 Replace with your UPI ID
      receiverName: "Dharmik",
      transactionRefId: "TID${DateTime.now().millisecondsSinceEpoch}",
      transactionNote: "Add Money to Wallet",
      amount: widget.amount,
    );

    switch (response.status) {
      case UpiPaymentStatus.SUCCESS:
        await _updateBalance();
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text("Payment Successful ✅")),
        );
        break;
      case UpiPaymentStatus.FAILURE:
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text("Payment Failed ❌")),
        );
        break;
      default:
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text("Payment Cancelled ⚠️")),
        );
        break;
    }
  }

  Future<void> _updateBalance() async {
    try {
      String? userEmail = GlobalData.userEmail;

      if (userEmail != null) {
        FirebaseFirestore.instance
            .collection('UserData')
            .doc(userEmail)
            .update({
          'balance': FieldValue.increment(widget.amount),
        }).then((value) {
          print("Balance Updated!");
        }).catchError((error) {
          print("Failed to update balance: $error");
        });
      } else {
        print("User email is not available.");
      }
    } catch (e) {
      print("Error updating balance: $e");
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("Google Pay")),
      body: Center(
        child: _gpay == null
            ? Foundation.kIsWeb
                ? Text("UPI payments are not supported on the web.")
                : CircularProgressIndicator()
            : ElevatedButton.icon(
                icon: Image.memory(_gpay!.icon, height: 30, width: 30),
                label: Text("Pay with Google Pay"),
                onPressed: _initiateTransaction,
              ),
      ),
    );
  }
}

// Dummy ProductDetailsPage (replace with your own)

class ProductListWithFilterPagee extends StatefulWidget {
  @override
  _ProductListWithFilterPageStatee createState() =>
      _ProductListWithFilterPageStatee();
}

class _ProductListWithFilterPageStatee
    extends State<ProductListWithFilterPagee> {
  String? _selectedCategory;
  List<String> _categories = [];
  bool _isLoading = true;

  @override
  void initState() {
    super.initState();
    _fetchCategories();
  }

  Future<void> _fetchCategories() async {
    try {
      final snapshot =
          await FirebaseFirestore.instance.collection('categories').get();
      setState(() {
        _categories =
            snapshot.docs.map((doc) => doc['name'] as String).toList();
        _isLoading = false;
      });
    } catch (e) {
      setState(() {
        _isLoading = false;
      });
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Error loading categories: $e')),
      );
    }
  }

  Stream<List<DocumentSnapshot>> _getFilteredProducts() {
    return FirebaseFirestore.instance
        .collection('All')
        .orderBy('createdAt', descending: true)
        .snapshots()
        .map((snapshot) {
      if (_selectedCategory == null || _selectedCategory == 'All') {
        return snapshot.docs;
      } else {
        return snapshot.docs.where((doc) {
          final category = doc['category']?.toString().toLowerCase() ?? '';
          return category == _selectedCategory!.toLowerCase();
        }).toList();
      }
    });
  }

  @override
  Widget build(BuildContext context) {
    final primaryColor = Color(0xFF1F2937);
    final accentColor = Color(0xFFD4AF37);

    return Scaffold(
      appBar: AppBar(
        title: Text(
          "Products",
          style: TextStyle(color: Colors.white),
        ),
        backgroundColor: Color(0xFF6A452C),
        foregroundColor: Colors.white,
        elevation: 0,
      ),
      body: Container(
        decoration: BoxDecoration(
          gradient: LinearGradient(
            begin: Alignment.topCenter,
            end: Alignment.bottomCenter,
            colors: [Colors.amber[100]!, Colors.amber[50]!],
          ),
        ),
        child: _isLoading
            ? Center(
                child: CircularProgressIndicator(
                  valueColor: AlwaysStoppedAnimation<Color>(accentColor),
                ),
              )
            : Column(
                children: [
                  Padding(
                    padding: const EdgeInsets.all(16.0),
                    child: Card(
                      elevation: 3,
                      shape: RoundedRectangleBorder(
                        borderRadius: BorderRadius.circular(12),
                      ),
                      child: Padding(
                        padding: const EdgeInsets.symmetric(horizontal: 16.0),
                        child: DropdownButton<String>(
                          isExpanded: true,
                          hint: Text("Select Category"),
                          value: _selectedCategory,
                          onChanged: (String? newValue) {
                            setState(() {
                              _selectedCategory = newValue;
                            });
                          },
                          items: [
                            DropdownMenuItem(
                              value: 'All',
                              child: Text(
                                'All Categories',
                                style: TextStyle(color: accentColor),
                              ),
                            ),
                            ..._categories.map((category) {
                              return DropdownMenuItem(
                                value: category,
                                child: Text(
                                  category,
                                  style: TextStyle(color: accentColor),
                                ),
                              );
                            }).toList(),
                          ],
                          underline: SizedBox(), // Remove default underline
                          icon: Icon(Icons.arrow_drop_down, color: accentColor),
                        ),
                      ),
                    ),
                  ),
                  Expanded(
                    child: StreamBuilder<List<DocumentSnapshot>>(
                      stream: _getFilteredProducts(),
                      builder: (context, snapshot) {
                        if (snapshot.connectionState ==
                            ConnectionState.waiting) {
                          return Center(
                              child: CircularProgressIndicator(
                            valueColor:
                                AlwaysStoppedAnimation<Color>(accentColor),
                          ));
                        }

                        if (snapshot.hasError) {
                          return Center(
                              child: Text('Error: ${snapshot.error}',
                                  style: TextStyle(color: accentColor)));
                        }

                        if (!snapshot.hasData || snapshot.data!.isEmpty) {
                          return Center(
                              child: Text('No products found 😢',
                                  style: TextStyle(
                                      fontSize: 18, color: Colors.grey[700])));
                        }

                        final products = snapshot.data!;
                        return ListView.builder(
                          padding: EdgeInsets.all(8),
                          itemCount: products.length,
                          itemBuilder: (context, index) {
                            final data =
                                products[index].data() as Map<String, dynamic>;
                            return _buildProductCard(data);
                          },
                        );
                      },
                    ),
                  ),
                ],
              ),
      ),
    );
  }

  Widget _buildProductCard(Map<String, dynamic> data) {
    return GestureDetector(
      onTap: () {
        Navigator.push(
          context,
          MaterialPageRoute(
            builder: (_) => ProductDetailsPage(
              name: data['name'] ?? 'No Name',
              description: data['description'] ?? '',
              imageUrl: data['imageUrl'] ?? '',
              price: data['price'] ?? 0,
            ),
          ),
        );
      },
      child: Card(
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.circular(16),
        ),
        elevation: 5,
        margin: const EdgeInsets.symmetric(vertical: 8, horizontal: 8),
        child: Row(
          children: [
            // Product Image
            ClipRRect(
              borderRadius: BorderRadius.only(
                topLeft: Radius.circular(16),
                bottomLeft: Radius.circular(16),
              ),
              child: Image.network(
                data['imageUrl'] ?? '',
                width: 100,
                height: 100,
                fit: BoxFit.cover,
                loadingBuilder: (context, child, loadingProgress) {
                  if (loadingProgress == null) return child;
                  return Container(
                    width: 100,
                    height: 100,
                    color: Colors.grey[200],
                    child: Center(
                      child: CircularProgressIndicator(),
                    ),
                  );
                },
                errorBuilder: (context, error, stackTrace) => Container(
                  width: 100,
                  height: 100,
                  color: Colors.grey[300],
                  child: Icon(Icons.image_not_supported, color: Colors.grey),
                ),
              ),
            ),
            SizedBox(width: 12),

            // Product Details
            Expanded(
              child: Padding(
                padding:
                    const EdgeInsets.symmetric(vertical: 10, horizontal: 5),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      data['name'] ?? 'No Name',
                      style: TextStyle(
                        fontWeight: FontWeight.bold,
                        fontSize: 18,
                        color: Colors.amber[800],
                      ),
                    ),
                    SizedBox(height: 4),
                    Text(
                      data['description'] ?? '',
                      maxLines: 2,
                      overflow: TextOverflow.ellipsis,
                      style: TextStyle(color: Colors.grey[700]),
                    ),
                    SizedBox(height: 8),
                    Text(
                      '₹${data['price']?.toStringAsFixed(2) ?? '--'}',
                      style: TextStyle(
                        fontWeight: FontWeight.bold,
                        color: Colors.amber[800],
                        fontSize: 16,
                      ),
                    ),
                  ],
                ),
              ),
            ),
          ],
        ),
      ),
    );
  }
}

class GoldRateCalculator extends StatefulWidget {
  @override
  _GoldRateCalculatorState createState() => _GoldRateCalculatorState();
}

class _GoldRateCalculatorState extends State<GoldRateCalculator> {
  final TextEditingController _moneyController = TextEditingController();
  double _goldRate = 0.0;
  double _goldValue = 0.0;
  bool _isLoading = true;

  @override
  void initState() {
    super.initState();
    _fetchGoldRate();
  }

  Future<void> _fetchGoldRate() async {
    try {
      DocumentSnapshot doc = await FirebaseFirestore.instance
          .collection('goldRate')
          .doc('current')
          .get();

      if (doc.exists && doc['rate'] != null) {
        setState(() {
          _goldRate = doc['rate'].toDouble();
          _isLoading = false;
        });
      } else {
        setState(() {
          _goldRate = 5000.0; // Default fallback value
          _isLoading = false;
        });
      }
    } catch (e) {
      print("Error fetching gold rate: $e");
      setState(() {
        _goldRate = 5000.0; // Default fallback value
        _isLoading = false;
      });
    }
  }

  void _calculateGoldValue() {
    double money = double.tryParse(_moneyController.text) ?? 0;
    setState(() {
      _goldValue = money / _goldRate; // Calculate gold value
    });
  }

  @override
  Widget build(BuildContext context) {
    final accentColor = Color(0xFFD4AF37);

    return Scaffold(
      appBar: AppBar(
        title: Text(
          'Gold Rate Calculator',
          style: TextStyle(color: Colors.white),
        ),
        backgroundColor: Color(0xFF6A452C),
        foregroundColor: Colors.white,
      ),
      backgroundColor: Colors.amber[100],
      body: _isLoading
          ? Center(child: CircularProgressIndicator())
          : Padding(
              padding: const EdgeInsets.all(16.0),
              child: Column(
                crossAxisAlignment: CrossAxisAlignment.stretch,
                children: [
                  Card(
                    elevation: 4,
                    margin: EdgeInsets.only(bottom: 16),
                    shape: RoundedRectangleBorder(
                      borderRadius: BorderRadius.circular(12),
                    ),
                    child: Padding(
                      padding: EdgeInsets.all(16),
                      child: Row(
                        mainAxisAlignment: MainAxisAlignment.spaceBetween,
                        children: [
                          Text(
                            'Current Gold Rate:',
                            style: TextStyle(
                                fontSize: 16, fontWeight: FontWeight.bold),
                          ),
                          Text(
                            '₹${_goldRate.toStringAsFixed(2)}/g',
                            style: TextStyle(
                                fontSize: 18,
                                fontWeight: FontWeight.bold,
                                color: accentColor),
                          ),
                        ],
                      ),
                    ),
                  ),
                  SizedBox(height: 16),
                  Card(
                    elevation: 4,
                    shape: RoundedRectangleBorder(
                      borderRadius: BorderRadius.circular(12),
                    ),
                    child: Padding(
                      padding: EdgeInsets.all(16),
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: [
                          Text(
                            'Enter Amount',
                            style: TextStyle(
                              fontSize: 16,
                              fontWeight: FontWeight.bold,
                            ),
                          ),
                          SizedBox(height: 16),
                          TextField(
                            controller: _moneyController,
                            decoration: InputDecoration(
                              labelText: 'Amount in Rupees (₹)',
                              prefixIcon: Icon(Icons.currency_rupee),
                              border: OutlineInputBorder(
                                borderRadius: BorderRadius.circular(10),
                              ),
                            ),
                            keyboardType: TextInputType.number,
                          ),
                          SizedBox(height: 20),
                          SizedBox(
                            width: double.infinity,
                            child: ElevatedButton(
                              onPressed: _calculateGoldValue,
                              child: Padding(
                                padding: EdgeInsets.symmetric(vertical: 12),
                                child: Text(
                                  'Calculate Gold Value',
                                  style: TextStyle(fontSize: 16),
                                ),
                              ),
                              style: ElevatedButton.styleFrom(
                                shape: RoundedRectangleBorder(
                                  borderRadius: BorderRadius.circular(10),
                                ),
                              ),
                            ),
                          ),
                        ],
                      ),
                    ),
                  ),
                  SizedBox(height: 24),
                  Card(
                    elevation: 4,
                    shape: RoundedRectangleBorder(
                      borderRadius: BorderRadius.circular(12),
                    ),
                    child: Padding(
                      padding: const EdgeInsets.all(16.0),
                      child: Column(
                        children: [
                          Text(
                            'Gold Value',
                            style: TextStyle(
                              fontSize: 18,
                              fontWeight: FontWeight.bold,
                            ),
                          ),
                          SizedBox(height: 12),
                          Row(
                            mainAxisAlignment: MainAxisAlignment.center,
                            children: [
                              Icon(
                                Icons.monetization_on,
                                size: 28,
                                color: accentColor,
                              ),
                              SizedBox(width: 10),
                              Text(
                                '${_goldValue.toStringAsFixed(4)} grams',
                                style: TextStyle(
                                  fontSize: 24,
                                  fontWeight: FontWeight.bold,
                                  color: accentColor,
                                ),
                              ),
                            ],
                          ),
                          SizedBox(height: 8),
                          Text(
                            'At current rate of ₹${_goldRate.toStringAsFixed(2)}/g',
                            style: TextStyle(
                              color: Colors.grey[600],
                              fontSize: 14,
                            ),
                          ),
                        ],
                      ),
                    ),
                  ),
                ],
              ),
            ),
    );
  }
}

class ProfileScreen extends StatefulWidget {
  @override
  _ProfileScreenState createState() => _ProfileScreenState();
}

class _ProfileScreenState extends State<ProfileScreen>
    with SingleTickerProviderStateMixin {
  late AnimationController _animationController;
  late Animation<double> _fadeAnimation;
  bool _isLoading = true;
  Map<String, dynamic> _userData = {};

  @override
  void initState() {
    super.initState();
    _animationController = AnimationController(
      vsync: this,
      duration: Duration(milliseconds: 800),
    );
    _fadeAnimation = Tween<double>(begin: 0.0, end: 1.0).animate(
      CurvedAnimation(
        parent: _animationController,
        curve: Curves.easeOut,
      ),
    );
    _fetchUserData();
    _animationController.forward();
  }

  @override
  void dispose() {
    _animationController.dispose();
    super.dispose();
  }

  Future<void> _fetchUserData() async {
    String? email = UserSession.getEmail();

    if (email == null) {
      setState(() {
        _isLoading = false;
      });
      return;
    }

    try {
      DocumentSnapshot doc = await FirebaseFirestore.instance
          .collection('UserData')
          .doc(email)
          .get();

      if (doc.exists) {
        setState(() {
          _userData = doc.data() as Map<String, dynamic>;
          _isLoading = false;
        });
      } else {
        setState(() {
          _isLoading = false;
        });
      }
    } catch (e) {
      print('Error fetching user data: $e');
      setState(() {
        _isLoading = false;
      });
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Error loading profile data')),
      );
    }
  }

  Widget _buildUserInfoRow(String label, String value, IconData icon) {
    return Padding(
      padding: EdgeInsets.symmetric(vertical: 8),
      child: Row(
        children: [
          Container(
            padding: EdgeInsets.all(10),
            decoration: BoxDecoration(
              color: Colors.amber.withOpacity(0.1),
              borderRadius: BorderRadius.circular(10),
            ),
            child: Icon(icon, color: Colors.amber[800], size: 20),
          ),
          SizedBox(width: 16),
          Expanded(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text(
                  label,
                  style: TextStyle(
                    fontSize: 12,
                    color: Colors.grey[600],
                  ),
                ),
                SizedBox(height: 2),
                Text(
                  value,
                  style: TextStyle(
                    fontWeight: FontWeight.w500,
                    fontSize: 16,
                  ),
                ),
              ],
            ),
          ),
        ],
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    final accentColor = Color(0xFFD4AF37);
    final primaryColor = Color(0xFF1F2937);

    return Scaffold(
      appBar: AppBar(
        title: Text('My Profile', style: TextStyle(color: Colors.white)),
        backgroundColor: primaryColor,
        foregroundColor: Colors.white,
      ),
      drawer: NavBar(),
      body: _isLoading
          ? Center(
              child: CircularProgressIndicator(
              valueColor: AlwaysStoppedAnimation<Color>(accentColor),
            ))
          : Container(
              decoration: BoxDecoration(
                gradient: LinearGradient(
                  begin: Alignment.topCenter,
                  end: Alignment.bottomCenter,
                  colors: [Colors.amber[100]!, Colors.amber[50]!],
                ),
              ),
              child: SafeArea(
                child: FadeTransition(
                  opacity: _fadeAnimation,
                  child: SingleChildScrollView(
                    padding: EdgeInsets.all(16.0),
                    child: Column(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        // Profile header with user info
                        Card(
                          elevation: 4,
                          shape: RoundedRectangleBorder(
                            borderRadius: BorderRadius.circular(16),
                          ),
                          child: Padding(
                            padding: EdgeInsets.all(16),
                            child: Row(
                              children: [
                                CircleAvatar(
                                  radius: 40,
                                  backgroundColor: accentColor,
                                  child: Text(
                                    (_userData['name'] as String? ?? 'U')
                                        .substring(0, 1)
                                        .toUpperCase(),
                                    style: TextStyle(
                                      fontSize: 30,
                                      fontWeight: FontWeight.bold,
                                      color: Colors.white,
                                    ),
                                  ),
                                ),
                                SizedBox(width: 16),
                                Expanded(
                                  child: Column(
                                    crossAxisAlignment:
                                        CrossAxisAlignment.start,
                                    children: [
                                      Text(
                                        _userData['name'] ?? 'User',
                                        style: TextStyle(
                                          fontSize: 20,
                                          fontWeight: FontWeight.bold,
                                        ),
                                      ),
                                      SizedBox(height: 5),
                                      Text(
                                        _userData['email'] ?? 'No email',
                                        style: TextStyle(
                                          fontSize: 14,
                                          color: Colors.grey[600],
                                        ),
                                      ),
                                    ],
                                  ),
                                ),
                              ],
                            ),
                          ),
                        ),
                        SizedBox(height: 20),

                        // Account balance & gold holdings
                        Row(
                          children: [
                            Expanded(
                              child: Card(
                                elevation: 2,
                                color: Colors.blue.withOpacity(0.1),
                                shape: RoundedRectangleBorder(
                                  borderRadius: BorderRadius.circular(12),
                                ),
                                child: Padding(
                                  padding: EdgeInsets.all(16),
                                  child: Column(
                                    crossAxisAlignment:
                                        CrossAxisAlignment.start,
                                    children: [
                                      Row(
                                        children: [
                                          Icon(Icons.account_balance_wallet,
                                              color: Colors.blue),
                                          SizedBox(width: 8),
                                          Text(
                                            'Account Balance',
                                            style: TextStyle(
                                              fontWeight: FontWeight.bold,
                                              fontSize: 14,
                                            ),
                                          ),
                                        ],
                                      ),
                                      SizedBox(height: 10),
                                      Text(
                                        '₹${_userData['balance']?.toString() ?? '0'}',
                                        style: TextStyle(
                                          fontSize: 24,
                                          fontWeight: FontWeight.bold,
                                          color: Colors.blue[800],
                                        ),
                                      ),
                                    ],
                                  ),
                                ),
                              ),
                            ),
                            SizedBox(width: 12),
                            Expanded(
                              child: Card(
                                elevation: 2,
                                color: accentColor.withOpacity(0.1),
                                shape: RoundedRectangleBorder(
                                  borderRadius: BorderRadius.circular(12),
                                ),
                                child: Padding(
                                  padding: EdgeInsets.all(16),
                                  child: Column(
                                    crossAxisAlignment:
                                        CrossAxisAlignment.start,
                                    children: [
                                      Row(
                                        children: [
                                          Icon(Icons.diamond,
                                              color: accentColor),
                                          SizedBox(width: 8),
                                          Text(
                                            'Gold Holdings',
                                            style: TextStyle(
                                              fontWeight: FontWeight.bold,
                                              fontSize: 14,
                                            ),
                                          ),
                                        ],
                                      ),
                                      SizedBox(height: 10),
                                      Text(
                                        '${_userData['gold']?.toString() ?? '0'} g',
                                        style: TextStyle(
                                          fontSize: 24,
                                          fontWeight: FontWeight.bold,
                                          color: accentColor,
                                        ),
                                      ),
                                    ],
                                  ),
                                ),
                              ),
                            ),
                          ],
                        ),
                        SizedBox(height: 20),

                        // Personal Information section
                        Text(
                          'Personal Information',
                          style: TextStyle(
                            fontSize: 18,
                            fontWeight: FontWeight.bold,
                            color: primaryColor,
                          ),
                        ),
                        SizedBox(height: 10),
                        Card(
                          elevation: 2,
                          shape: RoundedRectangleBorder(
                            borderRadius: BorderRadius.circular(12),
                          ),
                          child: Padding(
                            padding: EdgeInsets.all(16),
                            child: Column(
                              children: [
                                _buildUserInfoRow(
                                  'Phone Number',
                                  _userData['number'] ?? 'Not provided',
                                  Icons.phone,
                                ),
                                Divider(),
                                _buildUserInfoRow(
                                  'Address',
                                  _userData['address'] ?? 'Not provided',
                                  Icons.home,
                                ),
                                Divider(),
                                _buildUserInfoRow(
                                  'Aadhaar Card',
                                  _userData['aadharCard'] ?? 'Not provided',
                                  Icons.badge,
                                ),
                                Divider(),
                                _buildUserInfoRow(
                                  'PAN Card',
                                  _userData['panCard'] ?? 'Not provided',
                                  Icons.card_membership,
                                ),
                              ],
                            ),
                          ),
                        ),
                        SizedBox(height: 20),

                        // Account actions
                        Text(
                          'Account Actions',
                          style: TextStyle(
                            fontSize: 18,
                            fontWeight: FontWeight.bold,
                            color: primaryColor,
                          ),
                        ),
                        SizedBox(height: 10),
                        Card(
                          elevation: 2,
                          shape: RoundedRectangleBorder(
                            borderRadius: BorderRadius.circular(12),
                          ),
                          child: Padding(
                            padding: EdgeInsets.symmetric(vertical: 8),
                            child: Column(
                              children: [
                                ListTile(
                                  leading: Icon(
                                    Icons.history,
                                    color: Colors.blue,
                                  ),
                                  title: Text('Transaction History'),
                                  trailing:
                                      Icon(Icons.arrow_forward_ios, size: 16),
                                  onTap: () {
                                    ScaffoldMessenger.of(context).showSnackBar(
                                      SnackBar(
                                          content: Text(
                                              'Transaction History coming soon')),
                                    );
                                  },
                                ),
                                Divider(height: 1),
                                ListTile(
                                  leading: Icon(
                                    Icons.lock_reset,
                                    color: Colors.orange,
                                  ),
                                  title: Text('Change Password'),
                                  trailing:
                                      Icon(Icons.arrow_forward_ios, size: 16),
                                  onTap: () {
                                    ScaffoldMessenger.of(context).showSnackBar(
                                      SnackBar(
                                          content: Text(
                                              'Change Password feature coming soon')),
                                    );
                                  },
                                ),
                                Divider(height: 1),
                                ListTile(
                                  leading: Icon(
                                    Icons.edit,
                                    color: Colors.green,
                                  ),
                                  title: Text('Edit Profile'),
                                  trailing:
                                      Icon(Icons.arrow_forward_ios, size: 16),
                                  onTap: () async {
                                    bool? result = await Navigator.push(
                                      context,
                                      MaterialPageRoute(
                                        builder: (context) => EditProfileScreen(
                                            userData: _userData),
                                      ),
                                    );
                                    if (result == true) {
                                      _fetchUserData();
                                    }
                                  },
                                ),
                                Divider(height: 1),
                                ListTile(
                                  leading: Icon(
                                    Icons.logout,
                                    color: Colors.red,
                                  ),
                                  title: Text('Logout'),
                                  trailing:
                                      Icon(Icons.arrow_forward_ios, size: 16),
                                  onTap: () {
                                    UserSession.clear();
                                    Navigator.pushAndRemoveUntil(
                                      context,
                                      MaterialPageRoute(
                                          builder: (context) => LoginPage()),
                                      (Route<dynamic> route) => false,
                                    );
                                  },
                                ),
                              ],
                            ),
                          ),
                        ),
                      ],
                    ),
                  ),
                ),
              ),
            ),
    );
  }
}

class EditProfileScreen extends StatefulWidget {
  final Map<String, dynamic> userData;

  const EditProfileScreen({Key? key, required this.userData}) : super(key: key);

  @override
  _EditProfileScreenState createState() => _EditProfileScreenState();
}

class _EditProfileScreenState extends State<EditProfileScreen> {
  final _formKey = GlobalKey<FormState>();
  late TextEditingController nameController;
  late TextEditingController phoneController;
  late TextEditingController addressController;
  bool _isLoading = false;

  @override
  void initState() {
    super.initState();
    // Initialize controllers with existing user data
    nameController = TextEditingController(text: widget.userData['name'] ?? '');
    phoneController =
        TextEditingController(text: widget.userData['number'] ?? '');
    addressController =
        TextEditingController(text: widget.userData['address'] ?? '');
  }

  @override
  void dispose() {
    nameController.dispose();
    phoneController.dispose();
    addressController.dispose();
    super.dispose();
  }

  Future<void> _updateProfile() async {
    if (!_formKey.currentState!.validate()) {
      return;
    }

    setState(() => _isLoading = true);

    try {
      String? email = UserSession.getEmail();
      if (email == null) {
        throw Exception("User not logged in");
      }

      // Update Firestore document
      await FirebaseFirestore.instance
          .collection('UserData')
          .doc(email)
          .update({
        'name': nameController.text.trim(),
        'number': phoneController.text.trim(),
        'address': addressController.text.trim(),
        'updatedAt': FieldValue.serverTimestamp(),
      });

      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Profile updated successfully!')),
      );

      Navigator.pop(context, true); // Return true to indicate success
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Error updating profile: ${e.toString()}')),
      );
    } finally {
      setState(() => _isLoading = false);
    }
  }

  @override
  Widget build(BuildContext context) {
    final accentColor = Color(0xFFD4AF37);
    final primaryColor = Color(0xFF1F2937);

    return Scaffold(
      appBar: AppBar(
        title: Text('Edit Profile', style: TextStyle(color: Colors.white)),
        backgroundColor: primaryColor,
        foregroundColor: Colors.white,
      ),
      body: Container(
        decoration: BoxDecoration(
          gradient: LinearGradient(
            begin: Alignment.topCenter,
            end: Alignment.bottomCenter,
            colors: [Colors.amber[100]!, Colors.amber[50]!],
          ),
        ),
        child: SingleChildScrollView(
          padding: EdgeInsets.all(20),
          child: Form(
            key: _formKey,
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.stretch,
              children: [
                // Profile picture header
                Center(
                  child: Column(
                    children: [
                      CircleAvatar(
                        radius: 50,
                        backgroundColor: accentColor,
                        child: Text(
                          (nameController.text.isNotEmpty
                                  ? nameController.text[0]
                                  : 'U')
                              .toUpperCase(),
                          style: TextStyle(
                            fontSize: 36,
                            fontWeight: FontWeight.bold,
                            color: Colors.white,
                          ),
                        ),
                      ),
                      SizedBox(height: 12),
                      Text(
                        widget.userData['email'] ?? 'No Email',
                        style: TextStyle(
                          fontSize: 16,
                          color: Colors.grey[600],
                        ),
                      ),
                    ],
                  ),
                ),
                SizedBox(height: 30),

                // Full Name
                TextFormField(
                  controller: nameController,
                  decoration: InputDecoration(
                    labelText: 'Full Name',
                    prefixIcon: Icon(Icons.person),
                    border: OutlineInputBorder(
                      borderRadius: BorderRadius.circular(12),
                    ),
                    filled: true,
                    fillColor: Colors.white,
                  ),
                  validator: (value) => value == null || value.isEmpty
                      ? 'Please enter your name'
                      : null,
                ),
                SizedBox(height: 20),

                // Phone Number
                TextFormField(
                  controller: phoneController,
                  decoration: InputDecoration(
                    labelText: 'Phone Number',
                    prefixIcon: Icon(Icons.phone),
                    border: OutlineInputBorder(
                      borderRadius: BorderRadius.circular(12),
                    ),
                    filled: true,
                    fillColor: Colors.white,
                  ),
                  keyboardType: TextInputType.phone,
                  validator: (value) => value == null || value.isEmpty
                      ? 'Please enter your phone number'
                      : null,
                ),
                SizedBox(height: 20),

                // Address
                TextFormField(
                  controller: addressController,
                  decoration: InputDecoration(
                    labelText: 'Address',
                    prefixIcon: Icon(Icons.home),
                    border: OutlineInputBorder(
                      borderRadius: BorderRadius.circular(12),
                    ),
                    filled: true,
                    fillColor: Colors.white,
                  ),
                  maxLines: 3,
                  validator: (value) => value == null || value.isEmpty
                      ? 'Please enter your address'
                      : null,
                ),
                SizedBox(height: 30),

                // Update button
                ElevatedButton(
                  onPressed: _isLoading ? null : _updateProfile,
                  style: ElevatedButton.styleFrom(
                    backgroundColor: accentColor,
                    foregroundColor: Colors.white,
                    padding: EdgeInsets.symmetric(vertical: 16),
                    shape: RoundedRectangleBorder(
                      borderRadius: BorderRadius.circular(12),
                    ),
                    elevation: 4,
                  ),
                  child: _isLoading
                      ? SizedBox(
                          height: 20,
                          width: 20,
                          child: CircularProgressIndicator(
                            valueColor:
                                AlwaysStoppedAnimation<Color>(Colors.white),
                            strokeWidth: 2,
                          ),
                        )
                      : Text(
                          'UPDATE PROFILE',
                          style: TextStyle(
                            fontSize: 16,
                            fontWeight: FontWeight.bold,
                          ),
                        ),
                ),

                SizedBox(height: 20),

                // Note about unchangeable fields
                Container(
                  padding: EdgeInsets.all(15),
                  decoration: BoxDecoration(
                    color: Colors.blue.withOpacity(0.1),
                    borderRadius: BorderRadius.circular(12),
                    border: Border.all(color: Colors.blue.shade200),
                  ),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Row(
                        children: [
                          Icon(Icons.info_outline, color: Colors.blue),
                          SizedBox(width: 10),
                          Text(
                            'Information',
                            style: TextStyle(
                              fontWeight: FontWeight.bold,
                              color: Colors.blue[800],
                            ),
                          ),
                        ],
                      ),
                      SizedBox(height: 8),
                      Text(
                        'Some information like Email, Aadhaar Card, and PAN Card details cannot be changed for security reasons. Please contact support if you need assistance.',
                        style: TextStyle(
                          color: Colors.blue[800],
                          fontSize: 14,
                        ),
                      ),
                    ],
                  ),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }
}
// Dummy ProfilePage

class AdminScreen extends StatefulWidget {
  const AdminScreen({Key? key}) : super(key: key);

  @override
  State<AdminScreen> createState() => _AdminScreenState();
}

class _AdminScreenState extends State<AdminScreen>
    with SingleTickerProviderStateMixin {
  @override
  Widget build(BuildContext context) {
    // Modern luxury color palette
    final primaryColor = Color(0xFF1F2937);
    final accentColor = Color(0xFFD4AF37); // Gold color
    final cardColor = Colors.white;
    final textColor = Colors.black87;

    return Scaffold(
      drawer: NavBarA(),
      appBar: AppBar(
        title: Row(
          children: [
            Icon(Icons.diamond_outlined, color: accentColor),
            Text(
              "Admin Dashboard",
              style: TextStyle(
                fontWeight: FontWeight.bold,
                letterSpacing: 0.5,
                color: Colors.white,
              ),
            ),
          ],
        ),
        backgroundColor: primaryColor,
        elevation: 0,
        actions: [
          IconButton(
            icon: Icon(Icons.notifications_outlined, color: accentColor),
            onPressed: () {},
          ),
          IconButton(
            icon: Icon(Icons.settings_outlined, color: accentColor),
            onPressed: () {},
          ),
          SizedBox(width: 8),
        ],
      ),
      body: Center(
        child: Text("Admin View"),
      ),
    );
  }
}

class AdminWithdrawRequestsPage extends StatefulWidget {
  @override
  _AdminWithdrawRequestsPageState createState() =>
      _AdminWithdrawRequestsPageState();
}

class _AdminWithdrawRequestsPageState extends State<AdminWithdrawRequestsPage> {
  Future<void> _showConfirmDialog(String docId) async {
    showDialog(
      context: context,
      builder: (context) {
        return AlertDialog(
          title: Text("Confirm Withdrawal"),
          content: Text("Are you sure you want to confirm this request?"),
          actions: [
            TextButton(
              onPressed: () => Navigator.of(context).pop(), // cancel
              child: Text("Cancel ❌"),
            ),
            ElevatedButton(
              onPressed: () async {
                Navigator.of(context).pop(); // Close the dialog
                await _confirmRequest(docId); // Proceed to delete
              },
              child: Text("Confirm ✅"),
            ),
          ],
        );
      },
    );
  }

  Future<void> _confirmRequest(String docId) async {
    try {
      await FirebaseFirestore.instance
          .collection('WithdrawRequests')
          .doc(docId)
          .delete();

      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Withdrawal request confirmed ✅')),
      );
    } catch (e) {
      print('Error confirming request: $e');
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Failed to confirm request ❌')),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(
          "Withdrawal Requests",
          style: TextStyle(color: Colors.white),
        ),
        foregroundColor: Colors.white,
      ),
      body: StreamBuilder<QuerySnapshot>(
        stream: FirebaseFirestore.instance
            .collection('WithdrawRequests')
            .orderBy('timestamp', descending: true)
            .snapshots(),
        builder: (context, snapshot) {
          if (snapshot.connectionState == ConnectionState.waiting)
            return Center(child: CircularProgressIndicator());

          if (!snapshot.hasData || snapshot.data!.docs.isEmpty)
            return Center(child: Text("No withdrawal requests"));

          return ListView(
            children: snapshot.data!.docs.map((doc) {
              var data = doc.data() as Map<String, dynamic>;

              return Card(
                margin: EdgeInsets.symmetric(horizontal: 16, vertical: 8),
                child: ListTile(
                  title: Text("₹${data['amount']} - ${data['name']}"),
                  subtitle: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text("Email: ${data['email']}"),
                      Text("Bank: ${data['bank']}"),
                      Text("A/C: ${data['accountNumber']}"),
                      Text("IFSC: ${data['ifsc']}"),
                      Text("Date: ${data['timestamp'].toDate()}"),
                    ],
                  ),
                  trailing: ElevatedButton(
                    onPressed: () => _showConfirmDialog(doc.id),
                    child: Text("Confirm"),
                  ),
                ),
              );
            }).toList(),
          );
        },
      ),
    );
  }
}

class NavBarA extends StatefulWidget {
  const NavBarA({Key? key}) : super(key: key);

  @override
  State<NavBarA> createState() => _NavBarAState();
}

class _NavBarAState extends State<NavBarA> with TickerProviderStateMixin {
  late AnimationController _tapAnimationController;
  late AnimationController _listAnimationController;
  late List<Animation<Offset>> _slideAnimations;
  int _selectedIndex = -1;
  final List<IconData> _icons = [
    Icons.image,
    Icons.add,
    Icons.attach_money,
    Icons.monetization_on,
    Icons.person,
    Icons.edit_document,
    Icons.logout,
  ];

  @override
  void initState() {
    super.initState();

    // Controller for tap animation
    _tapAnimationController = AnimationController(
      vsync: this,
      duration: Duration(milliseconds: 150),
    );

    // Controller for list animation
    _listAnimationController = AnimationController(
      vsync: this,
      duration: Duration(milliseconds: 800),
    );

    // Create staggered animations for list items
    _slideAnimations = List.generate(
      _icons.length,
      (index) => Tween<Offset>(
        begin: Offset(-1.0, 0.0),
        end: Offset.zero,
      ).animate(CurvedAnimation(
        parent: _listAnimationController,
        curve: Interval(
          index * (1.0 / _icons.length) * 0.6,
          (index + 1) * (1.0 / _icons.length),
          curve: Curves.easeOutCubic,
        ),
      )),
    );

    _listAnimationController.forward();
  }

  @override
  void dispose() {
    _tapAnimationController.dispose();
    _listAnimationController.dispose();
    super.dispose();
  }

  Widget _buildAnimatedListTile({
    required int index,
    required IconData icon,
    required String title,
    required VoidCallback onTap,
    required Animation<Offset> slideAnimation,
  }) {
    // Modern luxury color palette
    final accentColor = Color(0xFFD4AF37); // Gold color
    final primaryDark = Color(0xFF1F2937);

    return SlideTransition(
      position: slideAnimation,
      child: MouseRegion(
        onEnter: (_) => setState(() => _selectedIndex = index),
        onExit: (_) => setState(() => _selectedIndex = -1),
        child: AnimatedContainer(
          duration: Duration(milliseconds: 200),
          decoration: BoxDecoration(
            gradient: _selectedIndex == index
                ? LinearGradient(
                    colors: [
                      accentColor.withOpacity(0.3),
                      Colors.white,
                    ],
                    begin: Alignment.centerLeft,
                    end: Alignment.centerRight,
                  )
                : null,
            borderRadius: BorderRadius.circular(12.0),
          ),
          margin: EdgeInsets.symmetric(horizontal: 10, vertical: 4),
          child: ListTile(
            leading: AnimatedContainer(
              duration: Duration(milliseconds: 200),
              padding: EdgeInsets.all(8),
              decoration: BoxDecoration(
                color: _selectedIndex == index
                    ? accentColor
                    : primaryDark.withOpacity(0.1),
                borderRadius: BorderRadius.circular(10),
              ),
              child: Icon(
                icon,
                size: 22,
                color: _selectedIndex == index ? Colors.white : accentColor,
              ),
            ),
            title: Text(
              title,
              style: TextStyle(
                fontWeight: _selectedIndex == index
                    ? FontWeight.bold
                    : FontWeight.normal,
                color: _selectedIndex == index ? primaryDark : Colors.black87,
                letterSpacing: 0.3,
              ),
            ),
            onTap: () {
              _tapAnimationController.forward().then((_) {
                _tapAnimationController.reverse();
                onTap();
              });
            },
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(12),
            ),
          ),
        ),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    // Modern luxury color palette
    final primaryColor = Color(0xFF1F2937);
    final accentColor = Color(0xFFD4AF37); // Gold color

    // Navigation items with their actions
    final navItems = [
      {
        'title': "Manage Carousel Images",
        'action': () {
          Navigator.pop(context);
          Navigator.push(
            context,
            MaterialPageRoute(builder: (context) => ManageCarouselScreen()),
          );
        },
      },
      {
        'title': "Manage Products",
        'action': () {
          Navigator.pop(context);
          Navigator.push(
            context,
            MaterialPageRoute(builder: (context) => ManageProductPage()),
          );
        },
      },
      {
        'title': "Set Gold Rate",
        'action': () {
          Navigator.pop(context);
          Navigator.push(
            context,
            MaterialPageRoute(builder: (context) => SetGoldRateScreen()),
          );
        },
      },
      {
        'title': "Buy/Sell Gold List",
        'action': () {
          Navigator.pop(context);
          Navigator.push(
            context,
            MaterialPageRoute(builder: (context) => TransactionsScreen()),
          );
        },
      },
      {
        'title': "Withdrawal Requests",
        'action': () {
          Navigator.pop(context);
          Navigator.push(
            context,
            MaterialPageRoute(
                builder: (context) => AdminWithdrawRequestsPage()),
          );
        },
      },
      {
        'title': "Edit Terms & Conditions",
        'action': () {
          Navigator.pop(context);
          Navigator.push(
            context,
            MaterialPageRoute(builder: (context) => TermsConditionsScreen()),
          );
        },
      },
      {
        'title': "Logout",
        'action': () {
          UserSession.clear();
          Navigator.pop(context);
          Navigator.pushAndRemoveUntil(
            context,
            MaterialPageRoute(builder: (context) => LoginPage()),
            (Route<dynamic> route) => false,
          );
        },
      },
    ];

    return Drawer(
      elevation: 16.0,
      backgroundColor: Colors.white,
      child: Column(
        children: [
          // Custom drawer header with gold accents
          Container(
            decoration: BoxDecoration(
              gradient: LinearGradient(
                colors: [primaryColor, Color(0xFF111827)],
                begin: Alignment.topLeft,
                end: Alignment.bottomRight,
              ),
            ),
            child: SafeArea(
              child: Padding(
                padding: EdgeInsets.all(20),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Row(
                      children: [
                        CircleAvatar(
                          backgroundColor: accentColor,
                          radius: 32,
                          child: Icon(
                            Icons.admin_panel_settings,
                            size: 36,
                            color: Colors.white,
                          ),
                        ),
                        SizedBox(width: 16),
                        Column(
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: [
                            Text(
                              "Admin Panel",
                              style: TextStyle(
                                fontSize: 24,
                                fontWeight: FontWeight.bold,
                                color: Colors.white,
                                letterSpacing: 0.5,
                              ),
                            ),
                            SizedBox(height: 4),
                            Text(
                              UserSession.email ?? "Not logged in",
                              style: TextStyle(
                                fontSize: 14,
                                color: Colors.grey[300],
                              ),
                            ),
                          ],
                        )
                      ],
                    ),
                    SizedBox(height: 24),
                    Container(
                      padding:
                          EdgeInsets.symmetric(horizontal: 12, vertical: 8),
                      decoration: BoxDecoration(
                        color: accentColor.withOpacity(0.2),
                        borderRadius: BorderRadius.circular(8),
                      ),
                      child: Row(
                        mainAxisSize: MainAxisSize.min,
                        children: [
                          Icon(Icons.verified, color: accentColor, size: 18),
                          SizedBox(width: 8),
                          Text(
                            "Gold App Admin",
                            style: TextStyle(
                              color: accentColor,
                              fontWeight: FontWeight.bold,
                            ),
                          ),
                        ],
                      ),
                    ),
                  ],
                ),
              ),
            ),
          ),

          // Small divider with gold accent
          Container(
            height: 4,
            width: double.infinity,
            decoration: BoxDecoration(
              gradient: LinearGradient(
                colors: [accentColor.withOpacity(0.3), accentColor],
                begin: Alignment.centerLeft,
                end: Alignment.centerRight,
              ),
            ),
          ),

          // Menu items
          Expanded(
            child: Container(
              decoration: BoxDecoration(
                color: Colors.white,
              ),
              child: ListView.builder(
                padding: EdgeInsets.symmetric(vertical: 12),
                itemCount: _icons.length,
                itemBuilder: (context, index) {
                  // Add divider before Logout
                  if (index == _icons.length - 1) {
                    return Column(
                      children: [
                        Divider(
                          height: 24,
                          thickness: 1,
                          color: Colors.grey[200],
                          indent: 16,
                          endIndent: 16,
                        ),
                        _buildAnimatedListTile(
                          index: index,
                          icon: _icons[index],
                          title: navItems[index]['title'] as String,
                          onTap: navItems[index]['action'] as VoidCallback,
                          slideAnimation: _slideAnimations[index],
                        ),
                      ],
                    );
                  }

                  return _buildAnimatedListTile(
                    index: index,
                    icon: _icons[index],
                    title: navItems[index]['title'] as String,
                    onTap: navItems[index]['action'] as VoidCallback,
                    slideAnimation: _slideAnimations[index],
                  );
                },
              ),
            ),
          ),

          // App version footer with gold accents
          Container(
            padding: EdgeInsets.symmetric(vertical: 16, horizontal: 20),
            decoration: BoxDecoration(
              color: Colors.grey[100],
              border: Border(
                top: BorderSide(
                  color: accentColor.withOpacity(0.3),
                  width: 1,
                ),
              ),
            ),
            child: Row(
              mainAxisAlignment: MainAxisAlignment.spaceBetween,
              children: [
                Row(
                  children: [
                    Icon(Icons.verified_user, color: accentColor, size: 18),
                    SizedBox(width: 8),
                    Text(
                      'Admin Panel v1.0',
                      style: TextStyle(
                        color: primaryColor,
                        fontWeight: FontWeight.w500,
                        fontSize: 12,
                      ),
                    ),
                  ],
                ),
                Container(
                  padding: EdgeInsets.symmetric(horizontal: 8, vertical: 4),
                  decoration: BoxDecoration(
                    color: primaryColor.withOpacity(0.1),
                    borderRadius: BorderRadius.circular(12),
                  ),
                  child: Text(
                    'Gold',
                    style: TextStyle(
                      color: accentColor,
                      fontWeight: FontWeight.bold,
                      fontSize: 12,
                    ),
                  ),
                ),
              ],
            ),
          ),
        ],
      ),
    );
  }
}

class ManageCarouselScreen extends StatefulWidget {
  @override
  _ManageCarouselScreenState createState() => _ManageCarouselScreenState();
}

class _ManageCarouselScreenState extends State<ManageCarouselScreen> {
  List<String> _images = [];
  bool _isLoading = true;
  List<Uint8List> _pickedImages = [];

  @override
  void initState() {
    super.initState();
    _loadImages();
  }

  Future<void> _loadImages() async {
    try {
      final snapshot = await FirebaseFirestore.instance
          .collection('carouselImages')
          .orderBy('createdAt', descending: true)
          .get();

      List<String> images =
          snapshot.docs.map((doc) => doc['imageUrl'] as String).toList();

      setState(() {
        _images = images;
        _isLoading = false;
      });
    } catch (e) {
      print('Error loading images: $e');
      setState(() {
        _isLoading = false;
      });
    }
  }

  Future<void> _pickImages() async {
    final picked = await ImagePickerWeb.getMultiImagesAsBytes();
    if (picked != null && picked.isNotEmpty) {
      setState(() {
        _pickedImages = picked;
      });
    }
  }

  Future<void> _uploadImages() async {
    if (_pickedImages.isEmpty) return;

    setState(() {
      _isLoading = true;
    });

    try {
      for (Uint8List imgBytes in _pickedImages) {
        final ref = FirebaseStorage.instance
            .ref()
            .child('carousel_images')
            .child('${DateTime.now().millisecondsSinceEpoch}.jpg');

        await ref.putData(imgBytes);
        final url = await ref.getDownloadURL();

        await FirebaseFirestore.instance.collection('carouselImages').add({
          'imageUrl': url,
          'createdAt': FieldValue.serverTimestamp(),
        });
      }

      _pickedImages.clear();
      await _loadImages();

      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Images uploaded successfully!')),
      );
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Upload failed: $e')),
      );
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    final accentColor = Color(0xFFD4AF37);
    final primaryColor = Color(0xFF1F2937);

    return Scaffold(
      appBar: AppBar(
        title: Text('Manage Carousel Images',
            style: TextStyle(color: Colors.white)),
        backgroundColor: primaryColor,
        foregroundColor: Colors.white,
        elevation: 0,
      ),
      body: Container(
        decoration: BoxDecoration(
          gradient: LinearGradient(
            begin: Alignment.topCenter,
            end: Alignment.bottomCenter,
            colors: [primaryColor, Color(0xFF111827)],
          ),
        ),
        child: _isLoading
            ? Center(
                child: CircularProgressIndicator(
                    valueColor: AlwaysStoppedAnimation<Color>(accentColor)))
            : Column(
                children: [
                  Padding(
                    padding: EdgeInsets.all(16),
                    child: Row(
                      children: [
                        Expanded(
                          child: Text(
                            'Carousel Images',
                            style: TextStyle(
                              fontSize: 20,
                              fontWeight: FontWeight.bold,
                              color: Colors.white,
                            ),
                          ),
                        ),
                        ElevatedButton.icon(
                          onPressed: _pickImages,
                          icon: Icon(Icons.add_photo_alternate),
                          label: Text('Add Images'),
                          style: ElevatedButton.styleFrom(
                            backgroundColor: accentColor,
                            foregroundColor: Colors.white,
                            padding: EdgeInsets.symmetric(
                                horizontal: 16, vertical: 12),
                          ),
                        ),
                        if (_pickedImages.isNotEmpty) ...[
                          SizedBox(width: 8),
                          ElevatedButton.icon(
                            onPressed: _uploadImages,
                            icon: Icon(Icons.upload),
                            label: Text('Upload'),
                            style: ElevatedButton.styleFrom(
                              backgroundColor: Colors.green,
                              foregroundColor: Colors.white,
                            ),
                          ),
                        ]
                      ],
                    ),
                  ),
                  if (_pickedImages.isNotEmpty)
                    Container(
                      height: 120,
                      child: ListView.builder(
                        scrollDirection: Axis.horizontal,
                        itemCount: _pickedImages.length,
                        itemBuilder: (ctx, i) => Container(
                          width: 120,
                          margin: EdgeInsets.symmetric(horizontal: 8),
                          decoration: BoxDecoration(
                            border: Border.all(color: accentColor, width: 2),
                            borderRadius: BorderRadius.circular(8),
                          ),
                          child: Stack(
                            alignment: Alignment.topRight,
                            children: [
                              ClipRRect(
                                borderRadius: BorderRadius.circular(6),
                                child: Image.memory(
                                  _pickedImages[i],
                                  fit: BoxFit.cover,
                                  width: double.infinity,
                                ),
                              ),
                              IconButton(
                                icon: Icon(Icons.cancel, color: Colors.red),
                                onPressed: () {
                                  setState(() {
                                    _pickedImages.removeAt(i);
                                  });
                                },
                              ),
                            ],
                          ),
                        ),
                      ),
                    ),
                  Expanded(
                    child: _images.isEmpty
                        ? Center(
                            child: Text('No images found',
                                style: TextStyle(
                                    color: Colors.white70, fontSize: 18)),
                          )
                        : GridView.builder(
                            padding: EdgeInsets.all(16),
                            gridDelegate:
                                SliverGridDelegateWithFixedCrossAxisCount(
                              crossAxisCount: 2,
                              crossAxisSpacing: 16,
                              mainAxisSpacing: 16,
                              childAspectRatio: 3 / 2,
                            ),
                            itemCount: _images.length,
                            itemBuilder: (ctx, i) => Container(
                              decoration: BoxDecoration(
                                borderRadius: BorderRadius.circular(12),
                                boxShadow: [
                                  BoxShadow(
                                    color: Colors.black26,
                                    blurRadius: 6,
                                    offset: Offset(0, 2),
                                  ),
                                ],
                              ),
                              child: ClipRRect(
                                borderRadius: BorderRadius.circular(12),
                                child: Stack(
                                  fit: StackFit.expand,
                                  children: [
                                    Image.network(
                                      _images[i],
                                      fit: BoxFit.cover,
                                      errorBuilder: (_, __, ___) => Container(
                                        color: Colors.grey,
                                        child: Icon(Icons.error, size: 40),
                                      ),
                                    ),
                                    Positioned(
                                      top: 5,
                                      right: 5,
                                      child: Container(
                                        decoration: BoxDecoration(
                                          color: Colors.red,
                                          shape: BoxShape.circle,
                                        ),
                                        child: IconButton(
                                          icon: Icon(Icons.delete,
                                              color: Colors.white, size: 20),
                                          padding: EdgeInsets.zero,
                                          constraints: BoxConstraints(
                                              minHeight: 36, minWidth: 36),
                                          onPressed: () async {
                                            try {
                                              final query =
                                                  await FirebaseFirestore
                                                      .instance
                                                      .collection(
                                                          'carouselImages')
                                                      .where('imageUrl',
                                                          isEqualTo: _images[i])
                                                      .get();

                                              if (query.docs.isNotEmpty) {
                                                await FirebaseFirestore.instance
                                                    .collection(
                                                        'carouselImages')
                                                    .doc(query.docs.first.id)
                                                    .delete();

                                                setState(() {
                                                  _images.removeAt(i);
                                                });

                                                ScaffoldMessenger.of(context)
                                                    .showSnackBar(
                                                  SnackBar(
                                                      content: Text(
                                                          'Image deleted successfully')),
                                                );
                                              }
                                            } catch (e) {
                                              ScaffoldMessenger.of(context)
                                                  .showSnackBar(
                                                SnackBar(
                                                    content: Text(
                                                        'Error deleting image: $e')),
                                              );
                                            }
                                          },
                                        ),
                                      ),
                                    ),
                                  ],
                                ),
                              ),
                            ),
                          ),
                  ),
                ],
              ),
      ),
    );
  }
}

class TermsConditionsScreen extends StatefulWidget {
  @override
  _TermsConditionsScreenState createState() => _TermsConditionsScreenState();
}

class _TermsConditionsScreenState extends State<TermsConditionsScreen> {
  final _formKey = GlobalKey<FormState>();
  final _termsController = TextEditingController();
  bool _isLoading = false;
  final Color _accentColor = Color(0xFFD4AF37);
  final Color _primaryColor = Color(0xFF1F2937);

  @override
  void initState() {
    super.initState();
    _loadTerms();
  }

  @override
  void dispose() {
    _termsController.dispose();
    super.dispose();
  }

  Future<void> _loadTerms() async {
    setState(() => _isLoading = true);

    try {
      final doc = await FirebaseFirestore.instance
          .collection('appSettings')
          .doc('terms')
          .get();

      if (doc.exists) {
        // Handle both string and list formats
        final content = doc['content'];
        if (content is List) {
          // Convert bullet list to plain text with newlines
          _termsController.text = content
              .map((line) => line.toString().replaceFirst(RegExp(r'^•\s?'), ''))
              .join('\n');
        } else {
          _termsController.text = content?.toString() ?? '';
        }
      }
    } catch (e) {
      _showError('Error loading terms: $e');
    } finally {
      setState(() => _isLoading = false);
    }
  }

  Future<void> _saveTerms() async {
    if (!_formKey.currentState!.validate()) return;

    setState(() => _isLoading = true);

    try {
      // Save as bullet points list
      final lines = _termsController.text
          .split('\n')
          .where((line) => line.trim().isNotEmpty)
          .map((line) => '• ${line.trim()}')
          .toList();

      await FirebaseFirestore.instance
          .collection('appSettings')
          .doc('terms')
          .set({
        'content': lines,
        'updatedAt': FieldValue.serverTimestamp(),
      });

      _showSuccess('Terms & Conditions updated successfully');
    } catch (e) {
      _showError('Error saving terms: $e');
    } finally {
      setState(() => _isLoading = false);
    }
  }

  void _showSuccess(String message) {
    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(
        content: Text(message),
        backgroundColor: Colors.green,
      ),
    );
  }

  void _showError(String message) {
    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(
        content: Text(message),
        backgroundColor: Colors.red,
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(
          'Terms & Conditions',
          style: TextStyle(color: Colors.white),
        ),
        backgroundColor: _primaryColor,
        foregroundColor: Colors.white,
        elevation: 0,
        actions: [
          if (!_isLoading)
            IconButton(
              icon: Icon(Icons.refresh),
              onPressed: _loadTerms,
              tooltip: 'Refresh',
            ),
        ],
      ),
      body: Container(
        decoration: BoxDecoration(
          gradient: LinearGradient(
            begin: Alignment.topCenter,
            end: Alignment.bottomCenter,
            colors: [_primaryColor, Color(0xFF111827)],
          ),
        ),
        child: _isLoading
            ? Center(
                child: CircularProgressIndicator(
                  valueColor: AlwaysStoppedAnimation<Color>(_accentColor),
                ),
              )
            : _buildContent(),
      ),
    );
  }

  Widget _buildContent() {
    return Padding(
      padding: const EdgeInsets.all(16.0),
      child: Card(
        shape: RoundedRectangleBorder(
          borderRadius: BorderRadius.circular(15),
        ),
        elevation: 5,
        child: Padding(
          padding: const EdgeInsets.all(20.0),
          child: Form(
            key: _formKey,
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                _buildHeader(),
                SizedBox(height: 20),
                _buildTermsField(),
                SizedBox(height: 20),
                _buildSaveButton(),
              ],
            ),
          ),
        ),
      ),
    );
  }

  Widget _buildHeader() {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Text(
          'Edit Terms & Conditions',
          style: TextStyle(
            fontSize: 22,
            fontWeight: FontWeight.bold,
            color: _primaryColor,
          ),
        ),
        SizedBox(height: 4),
        Text(
          'Enter each term in a new line. They will be displayed as bullet points in the app.',
          style: TextStyle(
            color: Colors.grey[600],
            fontSize: 14,
          ),
        ),
      ],
    );
  }

  Widget _buildTermsField() {
    return Expanded(
      child: TextFormField(
        controller: _termsController,
        decoration: InputDecoration(
          border: OutlineInputBorder(),
          hintText: 'Enter terms and conditions here...\nOne per line',
          filled: true,
          fillColor: Colors.grey[100],
        ),
        maxLines: null,
        expands: true,
        textAlignVertical: TextAlignVertical.top,
        validator: (value) {
          if (value == null || value.trim().isEmpty) {
            return 'Please enter terms and conditions';
          }
          return null;
        },
      ),
    );
  }

  Widget _buildSaveButton() {
    return SizedBox(
      width: double.infinity,
      height: 50,
      child: ElevatedButton(
        onPressed: _isLoading ? null : _saveTerms,
        style: ElevatedButton.styleFrom(
          backgroundColor: _accentColor,
          foregroundColor: Colors.white,
          shape: RoundedRectangleBorder(
            borderRadius: BorderRadius.circular(8),
          ),
        ),
        child: _isLoading
            ? CircularProgressIndicator(
                valueColor: AlwaysStoppedAnimation<Color>(Colors.white),
              )
            : Text(
                'Save Terms & Conditions',
                style: TextStyle(
                  fontSize: 18,
                  fontWeight: FontWeight.bold,
                ),
              ),
      ),
    );
  }
}

class ManageProductPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(
          "Manage Product",
        ),
        foregroundColor: Colors.white,
        centerTitle: true,
      ),
      body: Padding(
        padding: const EdgeInsets.all(24.0),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            ElevatedButton.icon(
              icon: Icon(Icons.add),
              label: Text("Add Product"),
              style: ElevatedButton.styleFrom(
                padding: EdgeInsets.symmetric(horizontal: 40, vertical: 15),
                textStyle: TextStyle(fontSize: 18),
              ),
              onPressed: () {
                Navigator.push(
                  context,
                  MaterialPageRoute(builder: (context) => AddProductScreen()),
                );
              },
            ),
            SizedBox(height: 30),
            ElevatedButton.icon(
              icon: Icon(Icons.delete),
              label: Text("Delete Product"),
              style: ElevatedButton.styleFrom(
                backgroundColor: Colors.redAccent,
                padding: EdgeInsets.symmetric(horizontal: 40, vertical: 15),
                textStyle: TextStyle(fontSize: 18),
              ),
              onPressed: () {
                Navigator.push(
                  context,
                  MaterialPageRoute(builder: (context) => DeleteProductPage()),
                );
              },
            ),
          ],
        ),
      ),
    );
  }
}

class DeleteProductPage extends StatelessWidget {
  // Function to delete the image from Firebase Storage
  Future<void> deleteProductImage(String imagePath) async {
    try {
      // Get reference to the image file in Firebase Storage
      Reference imageRef = FirebaseStorage.instance.ref().child(imagePath);

      // Delete the image file
      await imageRef.delete();

      print("Image deleted successfully!");
    } catch (e) {
      print("Error deleting image: $e");
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(
          "Delete Product",
        ),
        foregroundColor: Colors.white,
      ),
      body: StreamBuilder<QuerySnapshot>(
        stream: FirebaseFirestore.instance.collection("All").snapshots(),
        builder: (context, snapshot) {
          if (snapshot.connectionState == ConnectionState.waiting)
            return Center(child: CircularProgressIndicator());

          if (!snapshot.hasData || snapshot.data!.docs.isEmpty)
            return Center(child: Text("No products found"));

          return ListView(
            children: snapshot.data!.docs.map((doc) {
              final data = doc.data() as Map<String, dynamic>;
              final docId = doc.id;

              return Card(
                margin: EdgeInsets.symmetric(horizontal: 16, vertical: 8),
                child: ListTile(
                  leading: data['imageUrl'] != null && data['imageUrl'] != ''
                      ? Image.network(data['imageUrl'], width: 50, height: 50)
                      : Icon(Icons.image_not_supported),
                  title: Text(data['name'] ?? 'No Name'),
                  subtitle: Text("₹${data['price']}"),
                  trailing: IconButton(
                    icon: Icon(Icons.delete, color: Colors.red),
                    onPressed: () async {
                      bool confirm = await showDialog(
                        context: context,
                        builder: (ctx) => AlertDialog(
                          title: Text("Confirm Delete"),
                          content: Text(
                              "Are you sure you want to delete this product?"),
                          actions: [
                            TextButton(
                              onPressed: () => Navigator.pop(ctx, false),
                              child: Text("Cancel"),
                            ),
                            TextButton(
                              onPressed: () => Navigator.pop(ctx, true),
                              child: Text("Delete",
                                  style: TextStyle(color: Colors.red)),
                            ),
                          ],
                        ),
                      );

                      if (confirm) {
                        // If imageUrl exists, delete the image from Firebase Storage
                        if (data['imageUrl'] != null &&
                            data['imageUrl'] != '') {
                          await deleteProductImage(data['imageUrl']);
                        }

                        // Delete the product document from Firestore
                        await FirebaseFirestore.instance
                            .collection("All")
                            .doc(docId)
                            .delete();

                        ScaffoldMessenger.of(context).showSnackBar(
                          SnackBar(content: Text("Product deleted")),
                        );
                      }
                    },
                  ),
                ),
              );
            }).toList(),
          );
        },
      ),
    );
  }
}

class AddProductScreen extends StatefulWidget {
  @override
  _AddProductScreenState createState() => _AddProductScreenState();
}

class _AddProductScreenState extends State<AddProductScreen> {
  final _formKey = GlobalKey<FormState>();
  final _nameController = TextEditingController();
  final _descriptionController = TextEditingController();
  final _priceController = TextEditingController();

  List<String> _categories = [];
  String? _selectedCategory;
  bool _isLoading = false;
  bool _isFetchingCategories = true;
  Uint8List? _webImage;

  @override
  void initState() {
    super.initState();
    _fetchCategories();
  }

  @override
  void dispose() {
    _nameController.dispose();
    _descriptionController.dispose();
    _priceController.dispose();
    super.dispose();
  }

  Future<void> _fetchCategories() async {
    try {
      final snapshot =
          await FirebaseFirestore.instance.collection('categories').get();
      final catList = snapshot.docs
          .map((doc) => (doc.data() as Map<String, dynamic>)['name'] as String)
          .toList();

      setState(() {
        _categories = ['All', ...catList];
        _isFetchingCategories = false;
      });
    } catch (e) {
      setState(() => _isFetchingCategories = false);
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Error fetching categories: $e')),
      );
    }
  }

  Future<void> _pickImage() async {
    final picker = ImagePicker();
    final pickedImage = await picker.pickImage(source: ImageSource.gallery);

    if (pickedImage != null) {
      final bytes = await pickedImage.readAsBytes();
      setState(() {
        _webImage = bytes;
      });
    }
  }

  Future<String?> _uploadImageToFirebase(Uint8List data) async {
    try {
      final fileName = Uuid().v4();
      final ref =
          FirebaseStorage.instance.ref().child('product_images/$fileName.jpg');
      await ref.putData(data);
      return await ref.getDownloadURL();
    } catch (e) {
      print('Image upload error: $e');
      return null;
    }
  }

  Future<void> _addProduct() async {
    if (!_formKey.currentState!.validate() || _selectedCategory == null) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Please fill all fields and select a category')),
      );
      return;
    }

    setState(() => _isLoading = true);

    String? imageUrl;

    if (_webImage != null) {
      imageUrl = await _uploadImageToFirebase(_webImage!);
      if (imageUrl == null) {
        setState(() => _isLoading = false);
        ScaffoldMessenger.of(context).showSnackBar(
          SnackBar(content: Text('Failed to upload image')),
        );
        return;
      }
    }

    try {
      final productData = {
        'name': _nameController.text.trim(),
        'description': _descriptionController.text.trim(),
        'price': double.parse(_priceController.text.trim()),
        'imageUrl': imageUrl ?? '',
        'category': _selectedCategory,
        'createdAt': FieldValue.serverTimestamp(),
      };

      // Add to the specific category collection
      await FirebaseFirestore.instance
          .collection(_selectedCategory!)
          .add(productData);

      // Also add to the "All" collection if necessary
      if (_selectedCategory != 'All') {
        await FirebaseFirestore.instance.collection('All').add(productData);
      }

      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('✅ Product added successfully')),
      );

      _formKey.currentState!.reset();
      setState(() {
        _selectedCategory = null;
        _webImage = null;
      });
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('❌ Error: ${e.toString()}')),
      );
    } finally {
      setState(() => _isLoading = false);
    }
  }

  @override
  Widget build(BuildContext context) {
    final accentColor = Color(0xFFD4AF37);
    final primaryColor = Color(0xFF1F2937);

    return Scaffold(
      appBar: AppBar(
        title: Text(
          'Add Product',
          style: TextStyle(color: Colors.white),
        ),
        backgroundColor: primaryColor,
        foregroundColor: Colors.white,
        elevation: 0,
      ),
      body: Container(
        decoration: BoxDecoration(
          gradient: LinearGradient(
            begin: Alignment.topCenter,
            end: Alignment.bottomCenter,
            colors: [primaryColor, Color(0xFF111827)],
          ),
        ),
        child: SafeArea(
          child: SingleChildScrollView(
            padding: EdgeInsets.all(16),
            child: _isFetchingCategories
                ? Center(child: CircularProgressIndicator())
                : Form(
                    key: _formKey,
                    child: Column(
                      crossAxisAlignment: CrossAxisAlignment.stretch,
                      children: [
                        Container(
                          padding: EdgeInsets.all(20),
                          decoration: BoxDecoration(
                            color: Colors.white,
                            borderRadius: BorderRadius.circular(10),
                            boxShadow: [
                              BoxShadow(
                                color: Colors.black26,
                                blurRadius: 10,
                                offset: Offset(0, 4),
                              ),
                            ],
                          ),
                          child: Column(
                            crossAxisAlignment: CrossAxisAlignment.start,
                            children: [
                              Text(
                                'Add New Product',
                                style: TextStyle(
                                  fontSize: 22,
                                  fontWeight: FontWeight.bold,
                                  color: primaryColor,
                                ),
                              ),
                              SizedBox(height: 20),

                              // Product Name
                              TextFormField(
                                controller: _nameController,
                                decoration: InputDecoration(
                                  labelText: 'Product Name',
                                  border: OutlineInputBorder(),
                                  prefixIcon: Icon(Icons.shopping_bag),
                                ),
                                validator: (value) =>
                                    value == null || value.isEmpty
                                        ? 'Please enter product name'
                                        : null,
                              ),
                              SizedBox(height: 16),

                              // Description
                              TextFormField(
                                controller: _descriptionController,
                                decoration: InputDecoration(
                                  labelText: 'Description',
                                  border: OutlineInputBorder(),
                                  prefixIcon: Icon(Icons.description),
                                ),
                                maxLines: 3,
                                validator: (value) =>
                                    value == null || value.isEmpty
                                        ? 'Please enter product description'
                                        : null,
                              ),
                              SizedBox(height: 16),

                              // Price
                              TextFormField(
                                controller: _priceController,
                                decoration: InputDecoration(
                                  labelText: 'Price (₹)',
                                  border: OutlineInputBorder(),
                                  prefixIcon: Icon(Icons.currency_rupee),
                                ),
                                keyboardType: TextInputType.number,
                                validator: (value) {
                                  if (value == null || value.isEmpty)
                                    return 'Please enter price';
                                  if (double.tryParse(value) == null)
                                    return 'Please enter a valid price';
                                  return null;
                                },
                              ),
                              SizedBox(height: 16),

                              // Category dropdown
                              DropdownButtonFormField<String>(
                                decoration: InputDecoration(
                                  labelText: 'Category',
                                  border: OutlineInputBorder(),
                                  prefixIcon: Icon(Icons.category),
                                ),
                                value: _selectedCategory,
                                items: [
                                  ..._categories.map((cat) => DropdownMenuItem(
                                        value: cat,
                                        child: Text(cat),
                                      )),
                                  DropdownMenuItem(
                                    value: 'add_new',
                                    child: Row(
                                      children: [
                                        Icon(Icons.add, color: Colors.blue),
                                        SizedBox(width: 6),
                                        Text("Add New Category",
                                            style:
                                                TextStyle(color: Colors.blue)),
                                      ],
                                    ),
                                  ),
                                ],
                                onChanged: (value) {
                                  if (value == 'add_new') {
                                    Navigator.push(
                                      context,
                                      MaterialPageRoute(
                                          builder: (context) =>
                                              CategoryManagerPage()),
                                    ).then((_) => _fetchCategories());
                                  } else {
                                    setState(() => _selectedCategory = value);
                                  }
                                },
                                validator: (value) => value == null
                                    ? 'Please select a category'
                                    : null,
                              ),
                              SizedBox(height: 20),

                              // Image picker
                              Text(
                                'Product Image',
                                style: TextStyle(
                                  fontSize: 16,
                                  fontWeight: FontWeight.bold,
                                ),
                              ),
                              SizedBox(height: 8),
                              InkWell(
                                onTap: _pickImage,
                                child: Container(
                                  height: 200,
                                  width: double.infinity,
                                  decoration: BoxDecoration(
                                    border: Border.all(color: Colors.grey),
                                    borderRadius: BorderRadius.circular(8),
                                  ),
                                  child: _webImage != null
                                      ? ClipRRect(
                                          borderRadius:
                                              BorderRadius.circular(7),
                                          child: Image.memory(
                                            _webImage!,
                                            fit: BoxFit.cover,
                                          ),
                                        )
                                      : Column(
                                          mainAxisAlignment:
                                              MainAxisAlignment.center,
                                          children: [
                                            Icon(
                                              Icons.add_photo_alternate,
                                              size: 50,
                                              color: accentColor,
                                            ),
                                            SizedBox(height: 8),
                                            Text(
                                              'Tap to select an image',
                                              style: TextStyle(
                                                  color: Colors.grey[600]),
                                            ),
                                          ],
                                        ),
                                ),
                              ),
                              SizedBox(height: 24),

                              // Submit button
                              SizedBox(
                                height: 50,
                                width: double.infinity,
                                child: ElevatedButton(
                                  onPressed: _isLoading ? null : _addProduct,
                                  style: ElevatedButton.styleFrom(
                                    backgroundColor: accentColor,
                                    foregroundColor: Colors.white,
                                    shape: RoundedRectangleBorder(
                                      borderRadius: BorderRadius.circular(8),
                                    ),
                                  ),
                                  child: _isLoading
                                      ? CircularProgressIndicator(
                                          valueColor:
                                              AlwaysStoppedAnimation<Color>(
                                                  Colors.white))
                                      : Text(
                                          'Add Product',
                                          style: TextStyle(
                                            fontSize: 18,
                                            fontWeight: FontWeight.bold,
                                          ),
                                        ),
                                ),
                              ),
                            ],
                          ),
                        ),
                      ],
                    ),
                  ),
          ),
        ),
      ),
    );
  }
}

class CategoryManagerPage extends StatefulWidget {
  @override
  _CategoryManagerPageState createState() => _CategoryManagerPageState();
}

class _CategoryManagerPageState extends State<CategoryManagerPage> {
  final TextEditingController _categoryController = TextEditingController();

  Future<void> _addCategory() async {
    final name = _categoryController.text.trim();
    if (name.isEmpty) return;

    await FirebaseFirestore.instance
        .collection('categories')
        .add({'name': name});
    _categoryController.clear();
    ScaffoldMessenger.of(context)
        .showSnackBar(SnackBar(content: Text('Category added')));
    setState(() {}); // refresh list
  }

  Future<void> _deleteCategory(String docId) async {
    await FirebaseFirestore.instance
        .collection('categories')
        .doc(docId)
        .delete();
    ScaffoldMessenger.of(context)
        .showSnackBar(SnackBar(content: Text('Category deleted')));
    setState(() {}); // refresh list
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(
          "Manage Categories",
          style: TextStyle(color: Colors.white),
        ),
        foregroundColor: Colors.white,
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            // Add category input
            Row(
              children: [
                Expanded(
                  child: TextField(
                    controller: _categoryController,
                    decoration: InputDecoration(hintText: "New Category"),
                  ),
                ),
                SizedBox(width: 10),
                ElevatedButton(
                  onPressed: _addCategory,
                  child: Text("Add"),
                )
              ],
            ),
            SizedBox(height: 20),

            // Category list
            Expanded(
              child: StreamBuilder<QuerySnapshot>(
                stream: FirebaseFirestore.instance
                    .collection('categories')
                    .snapshots(),
                builder: (context, snapshot) {
                  if (snapshot.connectionState == ConnectionState.waiting)
                    return Center(child: CircularProgressIndicator());

                  final docs = snapshot.data?.docs ?? [];

                  if (docs.isEmpty) return Text("No categories yet");

                  return ListView.builder(
                    itemCount: docs.length,
                    itemBuilder: (context, index) {
                      final cat = docs[index];
                      return ListTile(
                        title: Text(cat['name']),
                        trailing: IconButton(
                          icon: Icon(Icons.delete, color: Colors.red),
                          onPressed: () => _deleteCategory(cat.id),
                        ),
                      );
                    },
                  );
                },
              ),
            )
          ],
        ),
      ),
    );
  }
}

class SetGoldRateScreen extends StatefulWidget {
  @override
  _SetGoldRateScreenState createState() => _SetGoldRateScreenState();
}

class _SetGoldRateScreenState extends State<SetGoldRateScreen> {
  final _formKey = GlobalKey<FormState>();
  final rateController = TextEditingController();
  bool _isLoading = false;
  double? _currentRate;

  @override
  void initState() {
    super.initState();
    _fetchCurrentRate();
  }

  @override
  void dispose() {
    rateController.dispose();
    super.dispose();
  }

  Future<void> _fetchCurrentRate() async {
    setState(() {
      _isLoading = true;
    });

    try {
      final doc = await FirebaseFirestore.instance
          .collection('goldRate')
          .doc('current')
          .get();

      if (doc.exists) {
        setState(() {
          _currentRate = doc['rate']?.toDouble();
          rateController.text = _currentRate?.toString() ?? '';
        });
      }
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Error fetching rate: $e')),
      );
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  Future<void> _updateGoldRate() async {
    if (!_formKey.currentState!.validate()) return;

    setState(() {
      _isLoading = true;
    });

    try {
      final newRate = double.parse(rateController.text);

      // Save the new rate
      await FirebaseFirestore.instance
          .collection('goldRate')
          .doc('current')
          .set({
        'rate': newRate,
        'updatedAt': FieldValue.serverTimestamp(),
      });

      // Also save to history
      await FirebaseFirestore.instance.collection('goldRateHistory').add({
        'rate': newRate,
        'timestamp': FieldValue.serverTimestamp(),
      });

      setState(() {
        _currentRate = newRate;
      });

      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Gold rate updated successfully!')),
      );
    } catch (e) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Error updating rate: $e')),
      );
    } finally {
      setState(() {
        _isLoading = false;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    final accentColor = Color(0xFFD4AF37);
    final primaryColor = Color(0xFF1F2937);

    return Scaffold(
      appBar: AppBar(
        title: Text(
          'Set Gold Rate',
          style: TextStyle(color: Colors.white),
        ),
        backgroundColor: primaryColor,
        foregroundColor: Colors.white,
        elevation: 0,
      ),
      body: Container(
        decoration: BoxDecoration(
          gradient: LinearGradient(
            begin: Alignment.topCenter,
            end: Alignment.bottomCenter,
            colors: [primaryColor, Color(0xFF111827)],
          ),
        ),
        child: SafeArea(
          child: Padding(
            padding: EdgeInsets.all(16),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Card(
                  elevation: 5,
                  shape: RoundedRectangleBorder(
                    borderRadius: BorderRadius.circular(15),
                  ),
                  child: Padding(
                    padding: EdgeInsets.all(16),
                    child: Column(
                      crossAxisAlignment: CrossAxisAlignment.center,
                      children: [
                        Row(
                          mainAxisAlignment: MainAxisAlignment.center,
                          children: [
                            Icon(
                              Icons.monetization_on,
                              color: accentColor,
                              size: 40,
                            ),
                            SizedBox(width: 10),
                            Text(
                              'Current Gold Rate',
                              style: TextStyle(
                                fontSize: 22,
                                fontWeight: FontWeight.bold,
                                color: primaryColor,
                              ),
                            ),
                          ],
                        ),
                        SizedBox(height: 20),
                        _isLoading
                            ? CircularProgressIndicator(
                                valueColor:
                                    AlwaysStoppedAnimation<Color>(accentColor))
                            : Text(
                                _currentRate != null
                                    ? '₹${_currentRate!.toStringAsFixed(2)} per gram'
                                    : 'Not set',
                                style: TextStyle(
                                  fontSize: 28,
                                  fontWeight: FontWeight.bold,
                                  color: accentColor,
                                ),
                              ),
                        SizedBox(height: 20),
                        Form(
                          key: _formKey,
                          child: Column(
                            children: [
                              TextFormField(
                                controller: rateController,
                                decoration: InputDecoration(
                                  labelText: 'New Gold Rate (per gram)',
                                  border: OutlineInputBorder(),
                                  prefixIcon: Icon(Icons.currency_rupee),
                                  hintText: 'Enter new rate in rupees',
                                ),
                                keyboardType: TextInputType.number,
                                validator: (value) {
                                  if (value == null || value.isEmpty) {
                                    return 'Please enter a rate';
                                  }
                                  if (double.tryParse(value) == null) {
                                    return 'Please enter a valid number';
                                  }
                                  return null;
                                },
                              ),
                              SizedBox(height: 20),
                              SizedBox(
                                height: 50,
                                width: double.infinity,
                                child: ElevatedButton(
                                  onPressed:
                                      _isLoading ? null : _updateGoldRate,
                                  style: ElevatedButton.styleFrom(
                                    backgroundColor: accentColor,
                                    foregroundColor: Colors.white,
                                    shape: RoundedRectangleBorder(
                                      borderRadius: BorderRadius.circular(8),
                                    ),
                                  ),
                                  child: _isLoading
                                      ? CircularProgressIndicator(
                                          valueColor:
                                              AlwaysStoppedAnimation<Color>(
                                                  Colors.white))
                                      : Text(
                                          'Update Gold Rate',
                                          style: TextStyle(
                                            fontSize: 18,
                                            fontWeight: FontWeight.bold,
                                          ),
                                        ),
                                ),
                              ),
                            ],
                          ),
                        ),
                      ],
                    ),
                  ),
                ),
                SizedBox(height: 24),
                Text(
                  'Rate History',
                  style: TextStyle(
                    fontSize: 20,
                    fontWeight: FontWeight.bold,
                    color: Colors.white,
                  ),
                ),
                SizedBox(height: 16),
                Expanded(
                  child: Card(
                    elevation: 5,
                    shape: RoundedRectangleBorder(
                      borderRadius: BorderRadius.circular(15),
                    ),
                    child: StreamBuilder<QuerySnapshot>(
                      stream: FirebaseFirestore.instance
                          .collection('goldRateHistory')
                          .orderBy('timestamp', descending: true)
                          .limit(10)
                          .snapshots(),
                      builder: (context, snapshot) {
                        if (snapshot.connectionState ==
                            ConnectionState.waiting) {
                          return Center(child: CircularProgressIndicator());
                        }

                        if (snapshot.hasError) {
                          return Center(child: Text('Error loading history'));
                        }

                        final documents = snapshot.data?.docs ?? [];

                        if (documents.isEmpty) {
                          return Center(child: Text('No history available'));
                        }

                        return ListView.separated(
                          padding: EdgeInsets.all(16),
                          itemCount: documents.length,
                          separatorBuilder: (_, __) => Divider(),
                          itemBuilder: (context, index) {
                            final data =
                                documents[index].data() as Map<String, dynamic>;
                            final rate = data['rate']?.toDouble() ?? 0.0;
                            final timestamp = data['timestamp'] as Timestamp?;

                            return ListTile(
                              leading: CircleAvatar(
                                backgroundColor: accentColor.withOpacity(0.2),
                                child: Icon(
                                  Icons.history,
                                  color: accentColor,
                                ),
                              ),
                              title: Text(
                                '₹${rate.toStringAsFixed(2)} per gram',
                                style: TextStyle(
                                  fontWeight: FontWeight.bold,
                                ),
                              ),
                              subtitle: timestamp != null
                                  ? Text(
                                      'Updated on ${timestamp.toDate().day}/${timestamp.toDate().month}/${timestamp.toDate().year} at ${timestamp.toDate().hour}:${timestamp.toDate().minute.toString().padLeft(2, '0')}',
                                    )
                                  : Text('Date not available'),
                            );
                          },
                        );
                      },
                    ),
                  ),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }
}

class TransactionsScreen extends StatefulWidget {
  @override
  _TransactionsScreenState createState() => _TransactionsScreenState();
}

class _TransactionsScreenState extends State<TransactionsScreen> {
  final List<String> _transactionTypes = ['All', 'Buy', 'Sell'];
  String _selectedTransactionType = 'All';
  final Color _accentColor = Color(0xFFD4AF37);
  final Color _primaryColor = Color(0xFF1F2937);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(
          'Gold Transactions',
          style: TextStyle(color: Colors.white),
        ),
        backgroundColor: _primaryColor,
        foregroundColor: Colors.white,
        elevation: 0,
      ),
      body: Container(
        decoration: BoxDecoration(
          gradient: LinearGradient(
            begin: Alignment.topCenter,
            end: Alignment.bottomCenter,
            colors: [_primaryColor, Color(0xFF111827)],
          ),
        ),
        child: Padding(
          padding: const EdgeInsets.all(16.0),
          child: Column(
            children: [
              _buildTransactionFilter(),
              SizedBox(height: 16),
              Expanded(child: _buildTransactionList()),
            ],
          ),
        ),
      ),
    );
  }

  Widget _buildTransactionFilter() {
    return DropdownButtonFormField<String>(
      value: _selectedTransactionType,
      decoration: InputDecoration(
        labelText: 'Filter Transactions',
        border: OutlineInputBorder(),
        filled: true,
        fillColor: Colors.white,
      ),
      items: _transactionTypes
          .map((type) => DropdownMenuItem(
                value: type,
                child: Text(type),
              ))
          .toList(),
      onChanged: (value) {
        if (value != null) {
          setState(() {
            _selectedTransactionType = value;
          });
        }
      },
    );
  }

  Widget _buildTransactionList() {
    return StreamBuilder<QuerySnapshot>(
      stream: _selectedTransactionType == 'All'
          ? FirebaseFirestore.instance
              .collection('GoldTransactions')
              .orderBy('timestamp', descending: true)
              .snapshots()
          : FirebaseFirestore.instance
              .collection('GoldTransactions')
              .where('type', isEqualTo: _selectedTransactionType.toLowerCase())
              .orderBy('timestamp', descending: true)
              .snapshots(),
      builder: (context, snapshot) {
        if (snapshot.connectionState == ConnectionState.waiting) {
          return Center(
            child: CircularProgressIndicator(
              valueColor: AlwaysStoppedAnimation<Color>(_accentColor),
            ),
          );
        }

        if (!snapshot.hasData || snapshot.data!.docs.isEmpty) {
          return _buildEmptyState('No transactions found');
        }

        return ListView.builder(
          itemCount: snapshot.data!.docs.length,
          itemBuilder: (context, index) {
            final tx = snapshot.data!.docs[index];
            return _buildTransactionCard(tx);
          },
        );
      },
    );
  }

  Widget _buildTransactionCard(DocumentSnapshot txDoc) {
    final tx = txDoc.data() as Map<String, dynamic>;
    final isBuy = tx['type'].toString().toLowerCase() == 'buy';

    return Card(
      elevation: 4,
      margin: EdgeInsets.only(bottom: 16),
      shape: RoundedRectangleBorder(
        borderRadius: BorderRadius.circular(12),
      ),
      child: Padding(
        padding: EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceBetween,
              children: [
                Text(
                  'Transaction #${txDoc.id.substring(0, 8).toUpperCase()}',
                  style: TextStyle(
                    fontWeight: FontWeight.bold,
                    fontSize: 18,
                  ),
                ),
                Container(
                  padding: EdgeInsets.symmetric(horizontal: 12, vertical: 6),
                  decoration: BoxDecoration(
                    color: isBuy
                        ? Colors.green.withOpacity(0.2)
                        : Colors.red.withOpacity(0.2),
                    borderRadius: BorderRadius.circular(20),
                    border: Border.all(
                      color: isBuy ? Colors.green : Colors.red,
                    ),
                  ),
                  child: Text(
                    tx['type'].toString().toUpperCase(),
                    style: TextStyle(
                      color: isBuy ? Colors.green : Colors.red,
                      fontWeight: FontWeight.bold,
                      fontSize: 12,
                    ),
                  ),
                ),
              ],
            ),
            Divider(height: 24),
            _buildTransactionDetailRow(
              Icons.attach_money,
              'Amount: ₹${tx['total']?.toStringAsFixed(2) ?? '0.00'}',
              isAmount: true,
            ),
            _buildTransactionDetailRow(
              Icons.scale,
              'Weight: ${tx['grams']}g @ ₹${tx['rate']}/g',
            ),
            if (tx['email'] != null)
              _buildTransactionDetailRow(
                Icons.email,
                'User: ${tx['email']}',
              ),
            _buildTransactionDetailRow(
              Icons.calendar_today,
              _formatDate(tx['timestamp'] as Timestamp?),
            ),
            Divider(height: 24),
            OutlinedButton.icon(
              onPressed: () => _showTransactionDetails(txDoc.id),
              icon: Icon(Icons.visibility),
              label: Text('View Details'),
              style: OutlinedButton.styleFrom(
                foregroundColor: _accentColor,
                side: BorderSide(color: _accentColor),
              ),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildTransactionDetailRow(IconData icon, String text,
      {bool isAmount = false}) {
    return Padding(
      padding: EdgeInsets.only(bottom: 8),
      child: Row(
        children: [
          Icon(icon, color: Colors.grey),
          SizedBox(width: 8),
          Expanded(
            child: Text(
              text,
              style: TextStyle(
                fontSize: 16,
                color: isAmount ? _accentColor : null,
                fontWeight: isAmount ? FontWeight.bold : null,
              ),
            ),
          ),
        ],
      ),
    );
  }

  Widget _buildEmptyState(String message) {
    return Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          Icon(Icons.money_off, color: Colors.white70, size: 60),
          SizedBox(height: 16),
          Text(
            message,
            style: TextStyle(color: Colors.white70, fontSize: 18),
          ),
        ],
      ),
    );
  }

  void _showTransactionDetails(String txId) {
    // Implement transaction details navigation
    Navigator.push(
      context,
      MaterialPageRoute(
        builder: (context) => TransactionDetailsScreen(transactionId: txId),
      ),
    );
  }

  String _formatDate(Timestamp? timestamp) {
    if (timestamp == null) return 'Unknown date';
    final date = timestamp.toDate();
    return DateFormat('dd MMM yyyy, hh:mm a').format(date);
  }
}

class TransactionDetailsScreen extends StatelessWidget {
  final String transactionId;

  const TransactionDetailsScreen({required this.transactionId});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(
          'Transaction Details',
        ),
        backgroundColor: Color(0xFF1F2937),
        foregroundColor: Colors.white,
      ),
      body: FutureBuilder<DocumentSnapshot>(
        future: FirebaseFirestore.instance
            .collection('GoldTransactions')
            .doc(transactionId)
            .get(),
        builder: (context, snapshot) {
          if (snapshot.connectionState == ConnectionState.waiting) {
            return Center(child: CircularProgressIndicator());
          }

          if (!snapshot.hasData || !snapshot.data!.exists) {
            return Center(child: Text('Transaction not found'));
          }

          final tx = snapshot.data!.data() as Map<String, dynamic>;
          final isBuy = tx['type'].toString().toLowerCase() == 'buy';

          return Padding(
            padding: const EdgeInsets.all(16.0),
            child: Card(
              elevation: 4,
              child: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Text(
                      'Transaction Details',
                      style: TextStyle(
                        fontSize: 20,
                        fontWeight: FontWeight.bold,
                      ),
                    ),
                    Divider(),
                    _buildDetailItem(
                        'Type',
                        tx['type'].toString().toUpperCase(),
                        isBuy ? Colors.green : Colors.red),
                    _buildDetailItem('Amount',
                        '₹${tx['total']?.toStringAsFixed(2) ?? '0.00'}'),
                    _buildDetailItem('Gold Weight', '${tx['grams']} grams'),
                    _buildDetailItem('Rate', '₹${tx['rate']} per gram'),
                    if (tx['email'] != null)
                      _buildDetailItem('User', tx['email']),
                    _buildDetailItem(
                        'Date', _formatDate(tx['timestamp'] as Timestamp?)),
                    if (tx['notes'] != null)
                      _buildDetailItem('Notes', tx['notes']),
                  ],
                ),
              ),
            ),
          );
        },
      ),
    );
  }

  Widget _buildDetailItem(String label, String value, [Color? valueColor]) {
    return Padding(
      padding: const EdgeInsets.symmetric(vertical: 8.0),
      child: Row(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          SizedBox(
            width: 100,
            child: Text(
              label,
              style: TextStyle(
                fontWeight: FontWeight.bold,
                color: Colors.grey[600],
              ),
            ),
          ),
          Expanded(
            child: Text(
              value,
              style: TextStyle(
                color: valueColor ?? Colors.black,
              ),
            ),
          ),
        ],
      ),
    );
  }

  String _formatDate(Timestamp? timestamp) {
    if (timestamp == null) return 'Unknown date';
    final date = timestamp.toDate();
    return DateFormat('dd MMM yyyy, hh:mm a').format(date);
  }
}

# Book-Heaven

lib/
├── main.dart                 # Entry point of the app
├── screens/                  # Contains all the screens
│   ├── splash_screen.dart    # Splash Screen
│   ├── onboarding_screen.dart # Onboarding Screen
│   ├── login_screen.dart     # Login Screen
│   ├── register_screen.dart  # Registration Screen
│   ├── home_screen.dart      # Home Screen
│   ├── book_details_screen.dart # Book Details Screen
│   ├── cart_screen.dart      # Cart Screen
├── models/                   # Data models for the app
│   ├── book_model.dart       # Book data model
│   ├── user_model.dart       # User data model
├── services/                 # Logic and helpers
│   ├── local_storage.dart    # For storing and retrieving data locally
├── widgets/                  # Reusable widgets
│   ├── custom_button.dart    # Custom Button Widget
│   ├── book_card.dart        # Book Card Widget
│   ├── cart_item.dart        # Cart Item Widget
└── utils/                    # Utility files
    ├── constants.dart        # App constants (e.g., colors, styles)
    ├── theme.dart            # App-wide theme


lib/
├── main.dart                 # Entry point of the app

  import 'package:flutter/material.dart';
import 'screens/splash_screen.dart';
import 'utils/theme.dart';

void main() {
  runApp(const BookHeavenApp());
}

class BookHeavenApp extends StatelessWidget {
  const BookHeavenApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Book Heaven',
      theme: appTheme,
      home: const SplashScreen(),
      debugShowCheckedModeBanner: false,
    );
  }
}

├── screens/                  # Contains all the screens
│   ├── splash_screen.dart    # Splash Screen
│       ```dart
        import 'package:flutter/material.dart';
        import 'onboarding_screen.dart';

        class SplashScreen extends StatefulWidget {
          const SplashScreen({Key? key}) : super(key: key);

          @override
          State<SplashScreen> createState() => _SplashScreenState();
        }

        class _SplashScreenState extends State<SplashScreen> {
          @override
          void initState() {
            super.initState();
            Future.delayed(const Duration(seconds: 2), () {
              Navigator.pushReplacement(
                context,
                MaterialPageRoute(builder: (context) => const OnboardingScreen()),
              );
            });
          }

          @override
          Widget build(BuildContext context) {
            return Scaffold(
              body: Center(
                child: Column(
                  mainAxisAlignment: MainAxisAlignment.center,
                  children: const [
                    Icon(Icons.book, size: 100, color: Colors.blue),
                    SizedBox(height: 20),
                    Text(
                      "Book Heaven",
                      style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
                    ),
                  ],
                ),
              ),
            );
          }
        }
       ```
│   ├── onboarding_screen.dart # Onboarding Screen
│       ```dart
        import 'package:flutter/material.dart';
        import 'login_screen.dart';
        import 'register_screen.dart';

        class OnboardingScreen extends StatelessWidget {
          const OnboardingScreen({Key? key}) : super(key: key);

          @override
          Widget build(BuildContext context) {
            return Scaffold(
              body: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                  mainAxisAlignment: MainAxisAlignment.center,
                  children: [
                    Image.asset('assets/images/onboarding.png', height: 200),
                    const SizedBox(height: 20),
                    const Text(
                      "Your Bookish Soulmate Awaits!",
                      style: TextStyle(fontSize: 18, fontWeight: FontWeight.w600),
                      textAlign: TextAlign.center,
                    ),
                    const SizedBox(height: 40),
                    ElevatedButton(
                      onPressed: () {
                        Navigator.push(
                          context,
                          MaterialPageRoute(builder: (context) => const LoginScreen()),
                        );
                      },
                      child: const Text("Login"),
                      style: ElevatedButton.styleFrom(minimumSize: const Size(double.infinity, 50)),
                    ),
                    const SizedBox(height: 10),
                    OutlinedButton(
                      onPressed: () {
                        Navigator.push(
                          context,
                          MaterialPageRoute(builder: (context) => const RegisterScreen()),
                        );
                      },
                      child: const Text("Register"),
                      style: OutlinedButton.styleFrom(minimumSize: const Size(double.infinity, 50)),
                    ),
                  ],
                ),
              ),
            );
          }
        }
       ```
│   ├── login_screen.dart     # Login Screen
│       ```dart
        import 'package:flutter/material.dart';
        import '../services/local_storage.dart';
        import 'home_screen.dart';

        class LoginScreen extends StatefulWidget {
          const LoginScreen({Key? key}) : super(key: key);

          @override
          State<LoginScreen> createState() => _LoginScreenState();
        }

        class _LoginScreenState extends State<LoginScreen> {
          final TextEditingController _emailController = TextEditingController();
          final TextEditingController _passwordController = TextEditingController();
          String? _errorMessage;

          void _login() async {
            final user = await LocalStorage.getUser();
            if (user['email'] == _emailController.text &&
                user['password'] == _passwordController.text) {
              Navigator.pushReplacement(
                context,
                MaterialPageRoute(builder: (context) => const HomeScreen()),
              );
            } else {
              setState(() {
                _errorMessage = 'Invalid email or password';
              });
            }
          }

          @override
          Widget build(BuildContext context) {
            return Scaffold(
              appBar: AppBar(title: const Text("Login")),
              body: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.stretch,
                  children: [
                    TextField(
                      controller: _emailController,
                      decoration: const InputDecoration(labelText: "Email"),
                    ),
                    const SizedBox(height: 10),
                    TextField(
                      controller: _passwordController,
                      decoration: const InputDecoration(labelText: "Password"),
                      obscureText: true,
                    ),
                    if (_errorMessage != null)
                      Padding(
                        padding: const EdgeInsets.only(top: 10),
                        child: Text(
                          _errorMessage!,
                          style: const TextStyle(color: Colors.red),
                        ),
                      ),
                    const SizedBox(height: 20),
                    ElevatedButton(
                      onPressed: _login,
                      child: const Text("Login"),
                    ),
                  ],
                ),
              ),
            );
          }
        }
       ```
│   ├── register_screen.dart  # Registration Screen
│       ```dart
        import 'package:flutter/material.dart';
        import '../services/local_storage.dart';

        class RegisterScreen extends StatefulWidget {
          const RegisterScreen({Key? key}) : super(key: key);

          @override
          State<RegisterScreen> createState() => _RegisterScreenState();
        }

        class _RegisterScreenState extends State<RegisterScreen> {
          final TextEditingController _emailController = TextEditingController();
          final TextEditingController _passwordController = TextEditingController();
          String? _errorMessage;

          void _register() async {
            if (_emailController.text.isNotEmpty && _passwordController.text.isNotEmpty) {
              await LocalStorage.saveUser(_emailController.text, _passwordController.text);
              Navigator.pop(context);
            } else {
              setState(() {
                _errorMessage = 'Please fill out all fields';
              });
            }
          }

          @override
          Widget build(BuildContext context) {
            return Scaffold(
              appBar: AppBar(title: const Text("Register")),
              body: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.stretch,
                  children: [
                    TextField(
                      controller: _emailController,
                      decoration: const InputDecoration(labelText: "Email"),
                    ),
                    const SizedBox(height: 10),
                    TextField(
                      controller: _passwordController,
                      decoration: const InputDecoration(labelText: "Password"),
                      obscureText: true,
                    ),
                    if (_errorMessage != null)
                      Padding(
                        padding: const EdgeInsets.only(top: 10),
                        child: Text(
                          _errorMessage!,
                          style: const TextStyle(color: Colors.red),
                        ),
                      ),
                    const SizedBox(height: 20),
                    ElevatedButton(
                      onPressed: _register,
                      child: const Text("Register"),
                    ),
                  ],
                ),
              ),
            );
          }
        }
       ```
│   ├── home_screen.dart      # Home Screen
│       ```dart
        import 'package:flutter/material.dart';
        import 'book_details_screen.dart';
        import 'cart_screen.dart';

        class HomeScreen extends StatelessWidget {
          const HomeScreen({Key? key}) : super(key: key);

          @override
          Widget build(BuildContext context) {
            return Scaffold(
              appBar: AppBar(
                title: const Text("Book Heaven"),
                actions: [
                  IconButton(
                    icon: const Icon(Icons.shopping_cart),
                    onPressed: () {
                      Navigator.push(
                        context,
                        MaterialPageRoute(builder: (context) => const CartScreen()),
                      );
                    },
                  )
                ],
              ),
              body: ListView.builder(
                itemCount: 10,
                itemBuilder: (context, index) {
                  return ListTile(
                    title: Text("Book $index"),
                    subtitle: const Text("Author Name"),
                    onTap: () {
                      Navigator.push(
                        context,
                        MaterialPageRoute(builder: (context) => const BookDetailsScreen()),
                      );
                    },
                  );
                },
              ),
            );
          }
        }
       ```
│   ├── book_details_screen.dart # Book Details Screen
│       ```dart
        import 'package:flutter/material.dart';

        class BookDetailsScreen extends StatelessWidget {
          const BookDetailsScreen({Key? key}) : super(key: key);

          @override
          Widget build(BuildContext context) {
            return Scaffold(
              appBar: AppBar(title: const Text("Book Details")),
              body: Padding(
                padding: const EdgeInsets.all(16.0),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: const [
                    Text("Book Title", style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold)),
                    SizedBox(height: 10),
                    Text("Author: Author Name"),
                    SizedBox(height: 10),
                    Text("Description: This is a great book about ..."),
                  ],
                ),
              ),
            );
          }
        }
       ```
│   ├── cart_screen.dart      # Cart Screen
│       ```dart
        import 'package:flutter/material.dart';

        class CartScreen extends StatelessWidget {
          const CartScreen({Key? key}) : super(key: key);

          @override
          Widget build(BuildContext context) {
            return Scaffold(
              appBar: AppBar(title: const Text("My Cart")),
              body: ListView.builder(
                itemCount: 5,
                itemBuilder: (context, index) {
                  return ListTile(
                    title: Text("Book $index"),
                    subtitle: const Text("Price: \$10"),
                    trailing: IconButton(
                      icon: const Icon(Icons.delete),
                      onPressed: () {},
                    ),
                  );
                },
              ),
              bottomNavigationBar: Padding(
                padding: const EdgeInsets.all(16.0),
                child: ElevatedButton(
                  onPressed: () {},
                  child: const Text("Checkout"),
                ),
              ),
            );
          }
        }
       ```
├── models/                   # Data models for the app
│   ├── book_model.dart       # Book data model
│       ```dart
        class Book {
          final String title;
          final String author;
          final String description;
          final double price;

          Book({
            required this.title,
            required this.author,
            required this.description,
            required this.price,
          });
        }
       ```
│   └── user_model.dart       # User data model
│       ```dart
        class User {
          final String email;
          final String password;

          User({
            required this.email,
            required this.password,
          });
        }
       ```
├── services/                 # Logic and helpers
│   └── local_storage.dart    # For storing and retrieving data locally
│       ```dart
        import 'package:shared_preferences/shared_preferences.dart';

        class LocalStorage {
          static const String _userEmailKey = 'user_email';
          static const String _userPasswordKey = 'user_password';

          static Future<void> saveUser(String email, String password) async {
            final prefs = await SharedPreferences.getInstance();
            await prefs.setString(_userEmailKey, email);
            await prefs.setString(_userPasswordKey, password);
          }

          static Future<Map<String, String>> getUser() async {
            final prefs = await SharedPreferences.getInstance();
            final email = prefs.getString(_userEmailKey) ?? '';
            final password = prefs.getString(_userPasswordKey) ?? '';
            return {'email': email, 'password': password};
          }
        }
       ```
├── widgets/                  # Reusable widgets
│   ├── custom_button.dart    # Custom Button Widget
│       ```dart
        import 'package:flutter/material.dart';

        class CustomButton extends StatelessWidget {
          final String text;
          final VoidCallback onPressed;

          const CustomButton({Key? key, required this.text, required this.onPressed}) : super(key: key);

          @override
          Widget build(BuildContext context) {
            return ElevatedButton(
              onPressed: onPressed,
              child: Text(text),
              style: ElevatedButton.styleFrom(minimumSize: const Size(double.infinity, 50)),
            );
          }
        }
       ```
│   ├── book_card.dart        # Book Card Widget
│       ```dart
        import 'package:flutter/material.dart';

        class BookCard extends StatelessWidget {
          final String title;
          final String author;
          final VoidCallback onTap;

          const BookCard({Key? key, required this.title, required this.author, required this.onTap}) : super(key: key);

          @override
          Widget build(BuildContext context) {
            return GestureDetector(
              onTap: onTap,
              child: Card(
                child: Padding(
                  padding: const EdgeInsets.all(16.0),
                  child: Column(
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text(title, style: const TextStyle(fontSize: 18, fontWeight: FontWeight.bold)),
                      const SizedBox(height: 5),
                      Text(author, style: const TextStyle(fontSize: 16, color: Colors.grey)),
                    ],
                  ),
                ),
              ),
            );
          }
        }
       ```
│   └── cart_item.dart       # Cart Item Widget
│       ```dart
        import 'package:flutter/material.dart';

        class CartItem extends StatelessWidget {
          final String title;
          final double price;
          final VoidCallback onRemove;

          const CartItem({Key? key, required this.title, required this.price, required this.onRemove}) : super(key: key);

          @override
          Widget build(BuildContext context) {
            return ListTile(
              title: Text(title),
              subtitle: Text("Price: \$${price.toStringAsFixed(2)}"),
              trailing: IconButton(
                icon: const Icon(Icons.delete),
                onPressed: onRemove,
              ),
            );
          }
        }
       ```
└── utils/                    # Utility files
    ├── constants.dart        # App constants (e.g., colors, styles)
    │       ```dart
            import 'package:flutter/material.dart';

            class AppColors {
              static const primaryColor = Colors.blue;
              static const secondaryColor = Colors.grey;
            }
       ```
    └── theme.dart            # App-wide theme
            ```dart
            import 'package:flutter/material.dart';

            ThemeData appTheme = ThemeData(
              primarySwatch: Colors.blue,
              visualDensity: VisualDensity.adaptivePlatformDensity,
            );
            ```

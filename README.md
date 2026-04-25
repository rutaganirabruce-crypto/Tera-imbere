import 'package:flutter/material.dart';

void main() {
  runApp(SavingsApp());
}

class SavingsApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      home: HomePage(),
    );
  }
}

class HomePage extends StatefulWidget {
  @override
  _HomePageState createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  String name = "Client";
  String language = "English";
  double savings = 0;
  double interestRate = 5;

  TextEditingController nameController = TextEditingController();
  TextEditingController savingsController = TextEditingController();

  double calculateInterest() {
    return savings * (interestRate / 100);
  }

  String translate(String text) {
    if (language == "Kinyarwanda") {
      if (text == "Hi") return "Muraho";
      if (text == "Savings") return "Ubwizigame";
    }
    if (language == "French") {
      if (text == "Hi") return "Bonjour";
      if (text == "Savings") return "Épargne";
    }
    return text;
  }

  @override
  Widget build(BuildContext context) {
    double interest = calculateInterest();

    return Scaffold(
      appBar: AppBar(
        title: Text("Savings App"),
      ),
      body: Padding(
        padding: EdgeInsets.all(16),
        child: ListView(
          children: [

            // Greeting
            Text(
              "${translate("Hi")} $name 👋",
              style: TextStyle(fontSize: 22, fontWeight: FontWeight.bold),
            ),

            SizedBox(height: 20),

            // Name input
            TextField(
              controller: nameController,
              decoration: InputDecoration(labelText: "Enter your name"),
              onChanged: (val) {
                setState(() {
                  name = val;
                });
              },
            ),

            SizedBox(height: 20),

            // Language selector
            DropdownButton<String>(
              value: language,
              items: ["English", "Kinyarwanda", "French"]
                  .map((lang) => DropdownMenuItem(
                        value: lang,
                        child: Text(lang),
                      ))
                  .toList(),
              onChanged: (val) {
                setState(() {
                  language = val!;
                });
              },
            ),

            SizedBox(height: 20),

            // Savings input
            TextField(
              controller: savingsController,
              keyboardType: TextInputType.number,
              decoration: InputDecoration(
                  labelText: translate("Savings")),
              onChanged: (val) {
                setState(() {
                  savings = double.tryParse(val) ?? 0;
                });
              },
            ),

            SizedBox(height: 20),

            // Interest display
            Text(
              "Interest: ${interest.toStringAsFixed(2)}",
              style: TextStyle(fontSize: 18),
            ),

            SizedBox(height: 20),

            // Simple Pie Chart (fake visual)
            Container(
              height: 150,
              child: CustomPaint(
                painter: PieChartPainter(savings, interest),
              ),
            ),

            SizedBox(height: 20),

            // AI Helper
            ElevatedButton(
              onPressed: () {
                showDialog(
                  context: context,
                  builder: (_) => AlertDialog(
                    title: Text("AI Helper"),
                    content: Text(
                        "Tip: Save at least 20% of your income monthly 💡"),
                  ),
                );
              },
              child: Text("Ask AI Helper"),
            ),

            SizedBox(height: 20),

            // Security (PIN placeholder)
            ElevatedButton(
              onPressed: () {
                showDialog(
                  context: context,
                  builder: (_) => AlertDialog(
                    title: Text("Security"),
                    content: Text("Set your PIN (coming soon) 🔒"),
                  ),
                );
              },
              child: Text("Security Settings"),
            ),
          ],
        ),
      ),
    );
  }
}

// Simple Pie Chart Painter
class PieChartPainter extends CustomPainter {
  final double savings;
  final double interest;

  PieChartPainter(this.savings, this.interest);

  @override
  void paint(Canvas canvas, Size size) {
    final paint = Paint()..style = PaintingStyle.fill;

    double total = savings + interest;
    if (total == 0) return;

    double savingsAngle = (savings / total) * 360;

    paint.color = Colors.blue;
    canvas.drawArc(
        Rect.fromLTWH(0, 0, size.width, size.height),
        0,
        savingsAngle * 3.14 / 180,
        true,
        paint);

    paint.color = Colors.green;
    canvas.drawArc(
        Rect.fromLTWH(0, 0, size.width, size.height),
        savingsAngle * 3.14 / 180,
        (360 - savingsAngle) * 3.14 / 180,
        true,
        paint);
  }

  @override
  bool shouldRepaint(covariant CustomPainter oldDelegate) => true;
}

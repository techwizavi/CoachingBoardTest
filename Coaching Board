import 'dart:async';
import 'package:flutter/material.dart';

void main() {
  runApp(const TacticalBoardApp());
}

class TacticalBoardApp extends StatelessWidget {
  const TacticalBoardApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Sports Coaching Board',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        primarySwatch: Colors.green,
        scaffoldBackgroundColor: Colors.grey[900],
      ),
      home: const BoardSelectionScreen(),
    );
  }
}

// 1. Sport & Board Selection Screen
class BoardSelectionScreen extends StatelessWidget {
  const BoardSelectionScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Select Coaching Board'),
        backgroundColor: Colors.green[800],
      ),
      body: Padding(
        padding: const EdgeInsets.all(24.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            const Text(
              'Choose a Sport:',
              style: TextStyle(color: Colors.white, fontSize: 22, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 20),
            _sportButton(context, 'Soccer', Icons.sports_soccer, Colors.green),
            _sportButton(context, 'Rugby', Icons.sports_rugby, Colors.amber),
            _sportButton(context, 'Futsal', Icons.sports_soccer, Colors.teal),
            _sportButton(context, 'Netball', Icons.sports_basketball, Colors.orange),
          ],
        ),
      ),
    );
  }

  Widget _sportButton(BuildContext context, String sportName, IconData icon, Color color) {
    return Padding(
      padding: const EdgeInsets.symmetric(vertical: 8.0),
      child: ElevatedButton.icon(
        style: ElevatedButton.styleFrom(
          backgroundColor: color,
          padding: const EdgeInsets.all(16),
          textStyle: const TextStyle(fontSize: 18, fontWeight: FontWeight.bold),
        ),
        icon: Icon(icon, color: Colors.white),
        label: Text('$sportName Board', style: const TextStyle(color: Colors.white)),
        onPressed: () {
          Navigator.push(
            context,
            MaterialPageRoute(
              builder: (context) => TacticsCanvasScreen(sportName: sportName),
            ),
          );
        },
      ),
    );
  }
}

// 2. Player Token Model
class PlayerToken {
  String id;
  Offset position;
  Color color;
  String label;

  PlayerToken({required this.id, required this.position, required this.color, required this.label});
}

// 3. The Interactive Tactics Board Screen with Animation
class TacticsCanvasScreen extends StatefulWidget {
  final String sportName;
  const TacticsCanvasScreen({super.key, required this.sportName});

  @override
  State<TacticsCanvasScreen> createState() => _TacticsCanvasScreenState();
}

class _TacticsCanvasScreenState extends State<TacticsCanvasScreen> {
  bool isHalfField = false;
  bool isRecording = false;
  bool isPlaying = false;
  
  final List<Map<String, Offset>> _recordedKeyframes = [];
  Timer? _playbackTimer;
  int _playbackIndex = 0;

  late List<PlayerToken> players;

  @override
  void initState() {
    super.initState();
    _initializePlayers();
  }

  void _initializePlayers() {
    if (widget.sportName == 'Netball') {
      // Netball: 7 players per side with official position tags
      players = [
        // Blue Team (Team A)
        PlayerToken(id: 'b_gk', position: const Offset(60, 250), color: Colors.blue, label: 'GK'),
        PlayerToken(id: 'b_gd', position: const Offset(150, 180), color: Colors.blue, label: 'GD'),
        PlayerToken(id: 'b_wd', position: const Offset(250, 120), color: Colors.blue, label: 'WD'),
        PlayerToken(id: 'b_c',  position: const Offset(380, 250), color: Colors.blue, label: 'C'),
        PlayerToken(id: 'b_wa', position: const Offset(480, 120), color: Colors.blue, label: 'WA'),
        PlayerToken(id: 'b_ga', position: const Offset(580, 180), color: Colors.blue, label: 'GA'),
        PlayerToken(id: 'b_gs', position: const Offset(680, 250), color: Colors.blue, label: 'GS'),

        // Yellow Team (Team B)
        PlayerToken(id: 'y_gs', position: const Offset(700, 250), color: Colors.yellow, label: 'GS'),
        PlayerToken(id: 'y_ga', position: const Offset(610, 320), color: Colors.yellow, label: 'GA'),
        PlayerToken(id: 'y_wa', position: const Offset(510, 380), color: Colors.yellow, label: 'WA'),
        PlayerToken(id: 'y_c',  position: const Offset(380, 280), color: Colors.yellow, label: 'C'),
        PlayerToken(id: 'y_wd', position: const Offset(280, 380), color: Colors.yellow, label: 'WD'),
        PlayerToken(id: 'y_gd', position: const Offset(180, 320), color: Colors.yellow, label: 'GD'),
        PlayerToken(id: 'y_gk', position: const Offset(80, 250), color: Colors.yellow, label: 'GK'),

        // Netball Equipment (Ball)
        PlayerToken(id: 'ball', position: const Offset(380, 210), color: Colors.white, label: '🏐'),
      ];
    } else {
      // Default Soccer / Futsal / Rugby setup
      players = [
        PlayerToken(id: 'blue_gk', position: const Offset(40, 250), color: Colors.pink, label: 'GK'),
        PlayerToken(id: 'b1', position: const Offset(120, 100), color: Colors.blue, label: '2'),
        PlayerToken(id: 'b2', position: const Offset(120, 200), color: Colors.blue, label: '3'),
        PlayerToken(id: 'b3', position: const Offset(120, 300), color: Colors.blue, label: '4'),
        PlayerToken(id: 'b4', position: const Offset(120, 400), color: Colors.blue, label: '5'),
        PlayerToken(id: 'b5', position: const Offset(220, 150), color: Colors.blue, label: '6'),
        PlayerToken(id: 'b6', position: const Offset(220, 250), color: Colors.blue, label: '7'),
        PlayerToken(id: 'b7', position: const Offset(220, 350), color: Colors.blue, label: '8'),
        PlayerToken(id: 'b8', position: const Offset(320, 180), color: Colors.blue, label: '9'),
        PlayerToken(id: 'b9', position: const Offset(320, 320), color: Colors.blue, label: '10'),
        PlayerToken(id: 'b10', position: const Offset(400, 250), color: Colors.blue, label: '11'),

        PlayerToken(id: 'yellow_gk', position: const Offset(740, 250), color: Colors.pink, label: 'GK'),
        PlayerToken(id: 'y1', position: const Offset(660, 100), color: Colors.yellow, label: '2'),
        PlayerToken(id: 'y2', position: const Offset(660, 200), color: Colors.yellow, label: '3'),
        PlayerToken(id: 'y3', position: const Offset(660, 300), color: Colors.yellow, label: '4'),
        PlayerToken(id: 'y4', position: const Offset(660, 400), color: Colors.yellow, label: '5'),
        PlayerToken(id: 'y5', position: const Offset(560, 150), color: Colors.yellow, label: '6'),
        PlayerToken(id: 'y6', position: const Offset(560, 250), color: Colors.yellow, label: '7'),
        PlayerToken(id: 'y7', position: const Offset(560, 350), color: Colors.yellow, label: '8'),
        PlayerToken(id: 'y8', position: const Offset(460, 180), color: Colors.yellow, label: '9'),
        PlayerToken(id: 'y9', position: const Offset(460, 320), color: Colors.yellow, label: '10'),
        PlayerToken(id: 'y10', position: const Offset(380, 250), color: Colors.yellow, label: '11'),

        PlayerToken(id: 'ball', position: const Offset(390, 250), color: Colors.white, label: '⚽'),
      ];
    }
    _recordedKeyframes.clear();
  }

  void _captureFrame() {
    if (isRecording) {
      final frameData = {for (var p in players) p.id: p.position};
      _recordedKeyframes.add(frameData);
    }
  }

  void _toggleRecording() {
    setState(() {
      isRecording = !isRecording;
      if (isRecording) {
        _recordedKeyframes.clear();
        _captureFrame();
      }
    });
    ScaffoldMessenger.of(context).showSnackBar(
      SnackBar(content: Text(isRecording ? 'Recording Play Started...' : 'Recording Stopped! Keyframes saved.')),
    );
  }

  void _playAnimation() {
    if (_recordedKeyframes.isEmpty) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('No recorded play found! Record a movement first.')),
      );
      return;
    }

    setState(() {
      isPlaying = true;
      _playbackIndex = 0;
    });

    _playbackTimer = Timer.periodic(const Duration(milliseconds: 50), (timer) {
      if (_playbackIndex < _recordedKeyframes.length) {
        final frame = _recordedKeyframes[_playbackIndex];
        setState(() {
          for (var player in players) {
            if (frame.containsKey(player.id)) {
              player.position = frame[player.id]!;
            }
          }
        });
        _playbackIndex++;
      } else {
        timer.cancel();
        setState(() {
          isPlaying = false;
        });
      }
    });
  }

  @override
  void dispose() {
    _playbackTimer?.cancel();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('${widget.sportName} (${isHalfField ? "Half" : "Full"})'),
        backgroundColor: Colors.green[800],
        actions: [
          IconButton(
            icon: Icon(isRecording ? Icons.fiber_manual_record : Icons.radio_button_unchecked),
            color: isRecording ? Colors.redAccent : Colors.white,
            tooltip: 'Record Play',
            onPressed: isPlaying ? null : _toggleRecording,
          ),
          IconButton(
            icon: const Icon(Icons.play_arrow),
            color: Colors.greenAccent,
            tooltip: 'Play Animation',
            onPressed: isRecording || isPlaying ? null : _playAnimation,
          ),
          IconButton(
            icon: Icon(isHalfField ? Icons.fullscreen : Icons.fullscreen_exit),
            tooltip: 'Toggle Field View',
            onPressed: isPlaying ? null : () => setState(() => isHalfField = !isHalfField),
          ),
          IconButton(
            icon: const Icon(Icons.refresh),
            tooltip: 'Reset Positions',
            onPressed: isPlaying ? null : _initializePlayers,
          ),
        ],
      ),
      body: Stack(
        children: [
          Positioned.frontFieldIfNeeded:
          Positioned.fill(
            child: CustomPaint(
              painter: FieldPainter(sport: widget.sportName, isHalf: isHalfField),
            ),
          ),
          ...players.map((player) {
            return Positioned(
              left: player.position.dx,
              top: player.position.dy,
              child: GestureDetector(
                onPanUpdate: isPlaying ? null : (details) {
                  setState(() {
                    player.position += details.delta;
                    _captureFrame();
                  });
                },
                child: Container(
                  width: 40,
                  height: 40,
                  decoration: BoxDecoration(
                    color: player.color,
                    shape: BoxShape.circle,
                    boxShadow: const [
                      BoxShadow(color: Colors.black45, blurRadius: 4, offset: Offset(0, 2))
                    ],
                    border: Border.all(color: Colors.black, width: 1.5),
                  ),
                  child: Center(
                    child: Text(
                      player.label,
                      style: TextStyle(
                        color: player.color == Colors.yellow ? Colors.black : Colors.white,
                        fontWeight: FontWeight.bold,
                        fontSize: 10,
                      ),
                    ),
                  ),
                ),
              ),
            );
          }),
        ],
      ),
    );
  }
}

// 4. Custom Painter for Field Markings (Soccer, Rugby, Futsal, Netball)
class FieldPainter extends CustomPainter {
  final String sport;
  final bool isHalf;

  FieldPainter({required this.sport, required this.isHalf});

  @override
  void paint(Canvas canvas, Size size) {
    final paint = Paint()
      ..color = Colors.white.withOpacity(0.85)
      ..strokeWidth = 3.0
      ..style = PaintingStyle.stroke;

    // Background Turf Color (Netball courts can be blue/green, keeping a clean professional look)
    final turfColor = sport == 'Netball' ? const Color(0xFF1565C0) : const Color(0xFF2E7D32);
    final turfPaint = Paint()..color = turfColor;
    canvas.drawRect(Rect.fromLTWH(0, 0, size.width, size.height), turfPaint);

    // Outer Boundary
    canvas.drawRect(Rect.fromLTWH(20, 20, size.width - 40, size.height - 40), paint);

    if (sport == 'Netball') {
      // Netball Court Markings: Thirds division lines
      double thirdWidth = (size.width - 40) / 3;
      
      // Transverse lines splitting the court into 3 thirds
      canvas.drawLine(Offset(20 + thirdWidth, 20), Offset(20 + thirdWidth, size.height - 20), paint);
      canvas.drawLine(Offset(20 + (thirdWidth * 2), 20), Offset(20 + (thirdWidth * 2), size.height - 20), paint);

      // Centre Circle
      canvas.drawCircle(Offset(size.width / 2, size.height / 2), 35, paint);
      
      // Goal Circles (Semi-circles at each end)
      canvas.drawArc(Rect.fromCircle(center: Offset(20, size.height / 2), radius: 70), -1.57, 3.14, false, paint);
      canvas.drawArc(Rect.fromCircle(center: Offset(size.width - 20, size.height / 2), radius: 70), 1.57, 3.14, false, paint);

    } else {
      // Standard Field (Soccer / Rugby / Futsal)
      if (!isHalf) {
        canvas.drawLine(
          Offset(size.width / 2, 20),
          Offset(size.width / 2, size.height - 20),
          paint,
        );
        canvas.drawCircle(Offset(size.width / 2, size.height / 2), 50, paint);
      }
    }
  }

  @override
  bool shouldRepaint(covariant CustomPainter oldDelegate) => true;
}

import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.ArrayList;
import java.util.List;
import java.util.Random;
import java.io.File;
import javax.sound.sampled.AudioInputStream;
import javax.sound.sampled.AudioSystem;
import javax.sound.sampled.Clip;

public class GodOfWarGame extends JPanel implements KeyListener {
    private int playerX, playerY;
    private static final int WIDTH = 800;
    private static final int HEIGHT = 600;
    private static final int PLAYER_SIZE = 50;
    private static final int PLAYER_SPEED = 5;
    private List<Enemy> enemies;
    private int score;
    private boolean gameOver;
    private Image background;
    private Clip backgroundMusic;
    private Clip gameOverSound;
    private int level;
    private int maxEnemies;
    private Font font;
    private Font gameOverFont;

    public GodOfWarGame() {
        setPreferredSize(new Dimension(WIDTH, HEIGHT));
        setFocusable(true);
        addKeyListener(this);
        playerX = WIDTH / 2;
        playerY = HEIGHT / 2;
        enemies = new ArrayList<>();
        score = 0;
        gameOver = false;
        background = new ImageIcon("background.png").getImage(); // Replace "background.png" with your image path
        playBackgroundMusic();
        level = 1;
        maxEnemies = 5;
        font = new Font("Arial", Font.BOLD, 20);
        gameOverFont = new Font("Arial", Font.BOLD, 40);
        spawnEnemies();
        gameLoop();
    }

    private void playBackgroundMusic() {
        try {
            AudioInputStream audioInputStream = AudioSystem.getAudioInputStream(new File("background_music.wav"));
            backgroundMusic = AudioSystem.getClip();
            backgroundMusic.open(audioInputStream);
            backgroundMusic.loop(Clip.LOOP_CONTINUOUSLY);
        } catch (Exception ex) {
            ex.printStackTrace();
        }
    }

    private void playGameOverSound() {
        try {
            AudioInputStream audioInputStream = AudioSystem.getAudioInputStream(new File("game_over.wav"));
            gameOverSound = AudioSystem.getClip();
            gameOverSound.open(audioInputStream);
            gameOverSound.start();
        } catch (Exception ex) {
            ex.printStackTrace();
        }
    }

    private void spawnEnemies() {
        Random rand = new Random();
        enemies.clear();
        for (int i = 0; i < maxEnemies; i++) {
            int x = rand.nextInt(WIDTH - PLAYER_SIZE);
            int y = rand.nextInt(HEIGHT - PLAYER_SIZE);
            enemies.add(new Enemy(x, y, level));
        }
    }

    private void gameLoop() {
        Timer timer = new Timer(1000 / 60, new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                if (!gameOver) {
                    update();
                    repaint();
                }
            }
        });
        timer.start();
    }

    private void update() {
        // Player movement
        if (playerX < 0) playerX = 0;
        if (playerX > WIDTH - PLAYER_SIZE) playerX = WIDTH - PLAYER_SIZE;
        if (playerY < 0) playerY = 0;
        if (playerY > HEIGHT - PLAYER_SIZE) playerY = HEIGHT - PLAYER_SIZE;

        // Enemy movement
        for (Enemy enemy : enemies) {
            enemy.move(playerX, playerY);
        }

        // Collision detection
        Rectangle playerRect = new Rectangle(playerX, playerY, PLAYER_SIZE, PLAYER_SIZE);
        for (Enemy enemy : enemies) {
            Rectangle enemyRect = new Rectangle(enemy.getX(), enemy.getY(), PLAYER_SIZE, PLAYER_SIZE);
            if (playerRect.intersects(enemyRect)) {
                gameOver = true;
                playGameOverSound();
                break;
            }
        }

        // Update score and check level progression
        score++;
        if (score % 100 == 0) {
            level++;
            maxEnemies += 2;
            spawnEnemies();
        }
    }

    public void paintComponent(Graphics g) {
        super.paintComponent(g);
        // Draw background
        g.drawImage(background, 0, 0, WIDTH, HEIGHT, null);
        // Draw player
        g.setColor(Color.RED);
        g.fillRect(playerX, playerY, PLAYER_SIZE, PLAYER_SIZE);
        // Draw enemies
        for (Enemy enemy : enemies) {
            g.setColor(Color.BLUE);
            g.fillRect(enemy.getX(), enemy.getY(), PLAYER_SIZE, PLAYER_SIZE);
        }
        // Draw score and level
        g.setColor(Color.WHITE);
        g.setFont(font);
        g.drawString("Score: " + score, 20, 30);
        g.drawString("Level: " + level, 20, 60);

        // Game over message
        if (gameOver) {
            g.setColor(Color.RED);
            g.setFont(gameOverFont);
            g.drawString("GAME OVER", WIDTH / 2 - 150, HEIGHT / 2);
        }
    }

    public void keyPressed(KeyEvent e) {
        int key = e.getKeyCode();
        if (key == KeyEvent.VK_LEFT) {
            playerX -= PLAYER_SPEED;
        }
        if (key == KeyEvent.VK_RIGHT) {
            playerX += PLAYER_SPEED;
        }
        if (key == KeyEvent.VK_UP) {
            playerY -= PLAYER_SPEED;
        }
        if (key == KeyEvent.VK_DOWN) {
            playerY += PLAYER_SPEED;
        }
    }

    public void keyReleased(KeyEvent e) {
    }

    public void keyTyped(KeyEvent e) {
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            public void run() {
                JFrame frame = new JFrame("God of War Java Game");
                GodOfWarGame game = new GodOfWarGame();
                frame.add(game);
                frame.pack();
                frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
                frame.setLocationRelativeTo(null);
                frame.setVisible(true);
            }
        });
    }

    class Enemy {
        private int x, y;
        private int speed;

        public Enemy(int x, int y, int level) {
            this.x = x;
            this.y = y;
            this.speed = level * 2;
        }

        public int getX() {
            return x;
        }

        public int getY() {
            return y;
        }

        public void move(int playerX, int playerY) {
            if (x < playerX) {
                x += speed;
            } else if (x > playerX) {
                x -= speed;
            }
            if (y < playerY) {
                y += speed;
            } else if (y > playerY) {
                y -= speed;
            }
        }
    }
}

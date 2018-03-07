import java.awt.*;
import java.awt.event.*;
import javax.swing.*;
import java.util.Random;
import java.io.BufferedReader;
import java.io.InputStreamReader;

public class GameOfLife extends JFrame {

    // Список констант и переменных
    final String NAME_OF_GAME = "Game of Life";
    final int WINDOW_LOCATION = 100;
    final int POINT_RADIUS = 10;
    final int GAME_SIZE = input();
    final int WINDOW_SIZE = GAME_SIZE*POINT_RADIUS+8;
    final int BTN_PANEL_HEIGHT = 60;
    int timeSleep = 1;

    boolean[][] tmp;
    volatile boolean goNextGeneration = false;
    Canvas canvasPanel;
    Random random = new Random();

    // Многомерные массивы для поколений
    boolean[][] oldGeneration = new boolean[GAME_SIZE][GAME_SIZE];
    boolean[][] newGeneration = new boolean[GAME_SIZE][GAME_SIZE];


    public static void main(String[] args) {
        new GameOfLife().go();
    }

    // Принимаем размер поля через консоль
    static int input () {
        int size = -1;
        BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
        while (size <= 0) {
            try {
                System.out.print("Введите размер поля: ");
                size = Integer.parseInt(reader.readLine());
                if (size < 1) {
                    throw new IllegalArgumentException();
                }
            } catch (Exception ex) {
                System.out.println("Введеное число меньше 1 или размер измеряется в натуральных числах!! Попробуйте ещё раз.");
                size = -1;
            }
        }
        return size;
    }

    void go() {

        // Создание окна, установка размеров окна и другое
        JFrame frame = new JFrame(NAME_OF_GAME);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(WINDOW_SIZE, WINDOW_SIZE + BTN_PANEL_HEIGHT);
        frame.setLocation(WINDOW_LOCATION, WINDOW_LOCATION);
        frame.setResizable(false);

        // Создание канвы
        canvasPanel = new Canvas();
        canvasPanel.setBackground(Color.WHITE);

        // Клавиша "RANDOM"
        JButton fillButton = new JButton("RANDOM");
        fillButton.addActionListener(new FillButtonListener());

        // Клавиша "NEXT"
        JButton stepButton = new JButton("NEXT");
        stepButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                processOfLife();
                canvasPanel.repaint();
            }
        });

        // Клавиша "PLAY/STOP"
        final JButton goButton = new JButton("PLAY");
        goButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                goNextGeneration = !goNextGeneration;
                goButton.setText(goNextGeneration? "STOP" : "PLAY");
                }
        });



        // Создание контейнера для компонентов
        JPanel buttonPanel = new JPanel();
        buttonPanel.add(fillButton);
        buttonPanel.add(stepButton);
        buttonPanel.add(goButton);

        // Установка местоположения компонентов
        frame.getContentPane().add(BorderLayout.CENTER, canvasPanel);
        frame.getContentPane().add(BorderLayout.SOUTH, buttonPanel);

        // Видимость ГПИ
        frame.setVisible(true);

        // Цикл для атоматической смены поколений
        while (true) {
            if (goNextGeneration) {
                processOfLife();
                canvasPanel.repaint();
                try {
                    Thread.sleep(200);
                } catch (InterruptedException e) { e.printStackTrace(); }
            }
        }
    }


    // Создаем случайное поколение после нажатия кнопки "RANDOM"
    public class FillButtonListener implements ActionListener {
        public void actionPerformed(ActionEvent ev) {
            //countGeneration = 1;
            for (int x = 0; x < GAME_SIZE; x++) {
                for (int y = 0; y < GAME_SIZE; y++) {
                    oldGeneration[x][y] = random.nextBoolean();
                }
            }
            canvasPanel.repaint();
        }
    }

    // Заполняем новый массив для нового поколения
    void processOfLife() {
        for (int x = 0; x < GAME_SIZE; x++) {
            for (int y = 0; y < GAME_SIZE; y++) {
                int count = countNeighbors(x, y);
                newGeneration[x][y] = oldGeneration[x][y];
                newGeneration[x][y] = (count == 3) ? true : newGeneration[x][y];
                newGeneration[x][y] = ((count < 2) || (count > 3)) ? false : newGeneration[x][y];
            }
        }
        tmp	= newGeneration;
        newGeneration = oldGeneration;
        oldGeneration = tmp;

        //countGeneration++;
    }

    // Считаем количество соседей
    int countNeighbors(int x, int y) {
        int count = 0;
        for (int dx = -1; dx < 2; dx++) {
            for (int dy = -1; dy < 2; dy++) {
                int nX = x + dx;
                int nY = y + dy;
                nX = (nX < 0) ? GAME_SIZE - 1 : nX;
                nY = (nY < 0) ? GAME_SIZE - 1 : nY;
                nX = (nX > GAME_SIZE - 1) ? 0 : nX;
                nY = (nY > GAME_SIZE - 1) ? 0 : nY;
                count += (oldGeneration[nX][nY]) ? 1 : 0;
            }
        }
        if (oldGeneration[x][y]) { count--; }
        return count;
    }

    // Переопределенный метод для отрисовки поколения на канве
    public class Canvas extends JPanel {
        @Override
        public void paint(Graphics g) {
            super.paint(g);
            for (int x = 0; x < GAME_SIZE; x++) {
                for (int y = 0; y < GAME_SIZE; y++) {
                    if (oldGeneration[x][y]) {
                        g.fillOval(x * POINT_RADIUS, y * POINT_RADIUS, POINT_RADIUS, POINT_RADIUS);
                    }
                }
            }
        }
    }
}

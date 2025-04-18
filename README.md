import javax.swing.*;
import java.awt.*;
import java.util.Arrays;

public class InterpolationLab {

    public static void main(String[] args) {
        // Задание 1: Интерполяция многочленом Лагранжа
        testLagrangeInterpolation();
        
        // Задание 2: Интерполяция кубическими сплайнами
        testSplineInterpolation();
        
        // Задание 2.2: Кубический сплайн для табличных данных
        testTableSplineInterpolation();
    }
    
    // Исходная функция f(x) = 1/(1 + 25x^2)
    public static double originalFunction(double x) {
        return 1.0 / (1.0 + 25 * x * x);
    }
    
    // 1. Интерполяция многочленом Лагранжа
    public static void testLagrangeInterpolation() {
        int[] nValues = {5, 10, 15}; // Разные значения n
        
        for (int n : nValues) {
            // Равноотстоящие узлы
            double[] xEq = new double[n+1];
            double[] yEq = new double[n+1];
            for (int i = 0; i <= n; i++) {
                xEq[i] = -1 + 2.0 * i / n;
                yEq[i] = originalFunction(xEq[i]);
            }
            
            // Чебышевские узлы
            double[] xCh = new double[n+1];
            double[] yCh = new double[n+1];
            for (int i = 0; i <= n; i++) {
                xCh[i] = Math.cos((2*i + 1) * Math.PI / (2*(n + 1)));
                yCh[i] = originalFunction(xCh[i]);
            }
            
            // Визуализация
            plotResults("Интерполяция Лагранжа (n=" + n + ") - равноотстоящие узлы", 
                       xEq, yEq, x -> lagrangeInterpolate(xEq, yEq, x));
            plotResults("Интерполяция Лагранжа (n=" + n + ") - чебышевские узлы", 
                       xCh, yCh, x -> lagrangeInterpolate(xCh, yCh, x));
        }
    }
    
    // Реализация интерполяции Лагранжа
    public static double lagrangeInterpolate(double[] x, double[] y, double point) {
        double result = 0;
        for (int i = 0; i < x.length; i++) {
            double term = y[i];
            for (int j = 0; j < x.length; j++) {
                if (j != i) {
                    term *= (point - x[j]) / (x[i] - x[j]);
                }
            }
            result += term;
        }
        return result;
    }
    
    // 2. Интерполяция кубическими сплайнами
    public static void testSplineInterpolation() {
        int[] nValues = {5, 10, 20}; // Разные значения n
        
        for (int n : nValues) {
            double[] x = new double[n+1];
            double[] y = new double[n+1];
            for (int i = 0; i <= n; i++) {
                x[i] = -1 + 2.0 * i / n;
                y[i] = originalFunction(x[i]);
            }
            
            CubicSpline spline = new CubicSpline(x, y);
            
            plotResults("Кубический сплайн (n=" + n + ")", x, y, spline::value);
        }
    }
    
    // 2.2 Кубический сплайн для табличных данных
    public static void testTableSplineInterpolation() {
        double[] x = {2, 3, 5, 7};
        double[] y = {4, -2, 6, -3};
        
        CubicSpline spline = new CubicSpline(x, y);
        
        // Проверка в узловых точках
        System.out.println("Проверка сплайна в узловых точках:");
        for (int i = 0; i < x.length; i++) {
            System.out.printf("f(%.1f) = %.4f (ожидалось %.4f)%n", 
                             x[i], spline.value(x[i]), y[i]);
        }
        
        plotResults("Кубический сплайн для табличных данных", x, y, spline::value);
    }
    
    // Класс для реализации кубического сплайна
    static class CubicSpline {
        private final double[] x;
        private final double[] y;
        private final double[] a;
        private final double[] b;
        private final double[] c;
        private final double[] d;
        
        public CubicSpline(double[] x, double[] y) {
            this.x = x;
            this.y = y;
            int n = x.length - 1;
            
            // Коэффициенты сплайна
            a = new double[n+1];
            b = new double[n];
            c = new double[n+1];
            d = new double[n];
            
            // Вычисление коэффициентов
            calculateCoefficients();
        }
        
        private void calculateCoefficients() {
            int n = x.length - 1;
            double[] h = new double[n];
            double[] alpha = new double[n];
            
            for (int i = 0; i < n; i++) {
                h[i] = x[i+1] - x[i];
            }
            
            for (int i = 1; i < n; i++) {
                alpha[i] = 3/h[i]*(y[i+1]-y[i]) - 3/h[i-1]*(y[i]-y[i-1]);
            }
            
            double[] l = new double[n+1];
            double[] mu = new double[n+1];
            double[] z = new double[n+1];
            
            l[0] = 1;
            mu[0] = 0;
            z[0] = 0;
            
            for (int i = 1; i < n; i++) {
                l[i] = 2*(x[i+1]-x[i-1]) - h[i-1]*mu[i-1];
                mu[i] = h[i]/l[i];
                z[i] = (alpha[i]-h[i-1]*z[i-1])/l[i];
            }
            
            l[n] = 1;
            z[n] = 0;
            c[n] = 0;
            
            for (int j = n-1; j >= 0; j--) {
                c[j] = z[j] - mu[j]*c[j+1];
                b[j] = (y[j+1]-y[j])/h[j] - h[j]*(c[j+1]+2*c[j])/3;
                d[j] = (c[j+1]-c[j])/(3*h[j]);
            }
            
            for (int j = 0; j < n; j++) {
                a[j] = y[j];
            }
        }
        
        public double value(double point) {
            if (point < x[0] || point > x[x.length-1]) {
                return 0;
            }
            
            int i = 0;
            while (i < x.length-1 && point > x[i+1]) {
                i++;
            }
            
            double dx = point - x[i];
            return a[i] + b[i]*dx + c[i]*dx*dx + d[i]*dx*dx*dx;
        }
    }
    
    // Метод для визуализации результатов
    public static void plotResults(String title, double[] xNodes, double[] yNodes, 
                                 java.util.function.DoubleUnaryOperator function) {
        JFrame frame = new JFrame(title);
        frame.setDefaultCloseOperation(JFrame.DISPOSE_ON_CLOSE);
        frame.setSize(800, 600);
        
        JPanel panel = new JPanel() {
            @Override
            protected void paintComponent(Graphics g) {
                super.paintComponent(g);
                Graphics2D g2 = (Graphics2D)g;
                g2.setRenderingHint(RenderingHints.KEY_ANTIALIASING, 
                                    RenderingHints.VALUE_ANTIALIAS_ON);
                
                int width = getWidth();
                int height = getHeight();
                int padding = 50;
                
                // Масштабирование
                double xMin = -1.1, xMax = 1.1;
                if (title.contains("табличных данных")) {
                    xMin = 1.5;
                    xMax = 7.5;
                }
                
                double yMin = -1.1, yMax = 1.1;
                if (title.contains("табличных данных")) {
                    yMin = -4;
                    yMax = 7;
                }
                
                // Оси координат
                g2.drawLine(padding, height-padding, width-padding, height-padding);
                g2.drawLine(padding, height-padding, padding, padding);
                
                // Подписи осей
                g2.drawString("X", width-padding+5, height-padding+15);
                g2.drawString("Y", padding-15, padding+5);
                
                // Оригинальная функция (только для не табличных данных)
                if (!title.contains("табличных данных")) {
                    g2.setColor(Color.BLUE);
                    int prevX = -1, prevY = -1;
                    for (int px = 0; px < width; px++) {
                        double x = xMin + (xMax - xMin) * px / (width - 1);
                        double y = originalFunction(x);
                        int py = height - padding - (int)((y - yMin) * (height - 2*padding) / (yMax - yMin));
                        
                        if (prevX >= 0) {
                            g2.drawLine(prevX, prevY, px, py);
                        }
                        prevX = px;
                        prevY = py;
                    }
                }
                
                // Интерполированная функция
                g2.setColor(Color.RED);
                int prevX = -1, prevY = -1;
                for (int px = 0; px < width; px++) {
                    double x = xMin + (xMax - xMin) * px / (width - 1);
                    double y = function.applyAsDouble(x);
                    int py = height - padding - (int)((y - yMin) * (height - 2*padding) / (yMax - yMin));
                    
                    if (prevX >= 0 && py >= padding && py <= height-padding) {
                        g2.drawLine(prevX, prevY, px, py);
                    }
                    prevX = px;
                    prevY = py;
                }
                
                // Узлы интерполяции
                g2.setColor(Color.GREEN);
                for (int i = 0; i < xNodes.length; i++) {
                    int px = padding + (int)((xNodes[i] - xMin) * (width - 2*padding) / (xMax - xMin));
                    int py = height - padding - (int)((yNodes[i] - yMin) * (height - 2*padding) / (yMax - yMin));
                    g2.fillOval(px-3, py-3, 6, 6);
                }
                
                // Легенда
                g2.setColor(Color.BLUE);
                if (!title.contains("табличных данных")) {
                    g2.drawString("Исходная функция", width - 150, padding + 20);
                }
                g2.setColor(Color.RED);
                g2.drawString("Интерполированная", width - 150, padding + 40);
                g2.setColor(Color.GREEN);
                g2.drawString("Узлы интерполяции", width - 150, padding + 60);
            }
        };
        
        frame.add(panel);
        frame.setVisible(true);
    }
}

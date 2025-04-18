import org.apache.commons.math3.analysis.polynomials.PolynomialFunction;
import org.apache.commons.math3.analysis.interpolation.SplineInterpolator;
import org.apache.commons.math3.analysis.interpolation.PolynomialInterpolator;
import org.apache.commons.math3.analysis.UnivariateFunction;
import java.util.Arrays;
import java.awt.*;
import javax.swing.*;

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
            double[] equidistantX = new double[n+1];
            double[] equidistantY = new double[n+1];
            for (int i = 0; i <= n; i++) {
                equidistantX[i] = -1 + 2.0 * i / n;
                equidistantY[i] = originalFunction(equidistantX[i]);
            }
            
            // Чебышевские узлы
            double[] chebyshevX = new double[n+1];
            double[] chebyshevY = new double[n+1];
            for (int i = 0; i <= n; i++) {
                chebyshevX[i] = Math.cos((2*i + 1) * Math.PI / (2*(n + 1)));
                chebyshevY[i] = originalFunction(chebyshevX[i]);
            }
            
            // Создаем интерполяционные полиномы
            PolynomialInterpolator interpolator = new PolynomialInterpolator();
            UnivariateFunction equidistantPoly = interpolator.interpolate(equidistantX, equidistantY);
            UnivariateFunction chebyshevPoly = interpolator.interpolate(chebyshevX, chebyshevY);
            
            // Визуализация
            plotResults("Интерполяция Лагранжа (n=" + n + ") - равноотстоящие узлы", 
                       equidistantX, equidistantY, equidistantPoly);
            plotResults("Интерполяция Лагранжа (n=" + n + ") - чебышевские узлы", 
                       chebyshevX, chebyshevY, chebyshevPoly);
        }
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
            
            SplineInterpolator splineInterpolator = new SplineInterpolator();
            UnivariateFunction spline = splineInterpolator.interpolate(x, y);
            
            plotResults("Кубический сплайн (n=" + n + ")", x, y, spline);
        }
    }
    
    // 2.2 Кубический сплайн для табличных данных
    public static void testTableSplineInterpolation() {
        double[] x = {2, 3, 5, 7};
        double[] y = {4, -2, 6, -3};
        
        SplineInterpolator splineInterpolator = new SplineInterpolator();
        UnivariateFunction spline = splineInterpolator.interpolate(x, y);
        
        // Проверка в узловых точках
        System.out.println("Проверка сплайна в узловых точках:");
        for (int i = 0; i < x.length; i++) {
            System.out.printf("f(%.1f) = %.4f (ожидалось %.4f)%n", 
                             x[i], spline.value(x[i]), y[i]);
        }
        
        plotResults("Кубический сплайн для табличных данных", x, y, spline);
    }
    
    // Метод для визуализации результатов
    public static void plotResults(String title, double[] xNodes, double[] yNodes, 
                                 UnivariateFunction interpolatedFunction) {
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
                    double y = interpolatedFunction.value(x);
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
            }
        };
        
        frame.add(panel);
        frame.setVisible(true);
    }
}

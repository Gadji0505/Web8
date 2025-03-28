public class LinearSystems {
    public static void main(String[] args) {
        // Первая система уравнений
        System.out.println("Решение первой системы:");
        solveFirstSystem();
        
        // Вторая система уравнений
        System.out.println("\nРешение второй системы:");
        solveSecondSystem();
    }
    
    // Функция для вычисления erf (пока упрощенная)
    public static double erf(double x) {
        // Это очень простой вариант, на практике нужно использовать точный расчет
        return x - (x*x*x)/3 + (x*x*x*x*x)/10;
    }
    
    public static void solveFirstSystem() {
        // Коэффициенты системы уравнений
        double[][] matrix = {
            {1.00, 0.80, 0.64},
            {1.00, 0.90, 0.81},
            {1.00, 1.10, 1.21}
        };
        
        // Правая часть уравнений
        double[] b = {
            erf(0.80),
            erf(0.90),
            erf(1.10)
        };
        
        // Попробуем решить систему методом Крамера
        double detA = determinant(matrix);
        
        if (Math.abs(detA) < 0.000001) {
            System.out.println("Система не имеет единственного решения");
            return;
        }
        
        // Матрицы для метода Крамера
        double[][] matrixX1 = replaceColumn(matrix, b, 0);
        double[][] matrixX2 = replaceColumn(matrix, b, 1);
        double[][] matrixX3 = replaceColumn(matrix, b, 2);
        
        double x1 = determinant(matrixX1) / detA;
        double x2 = determinant(matrixX2) / detA;
        double x3 = determinant(matrixX3) / detA;
        
        System.out.println("Решение:");
        System.out.println("x1 = " + x1);
        System.out.println("x2 = " + x2);
        System.out.println("x3 = " + x3);
        
        // Проверка невязки
        double res1 = matrix[0][0]*x1 + matrix[0][1]*x2 + matrix[0][2]*x3 - b[0];
        double res2 = matrix[1][0]*x1 + matrix[1][1]*x2 + matrix[1][2]*x3 - b[1];
        double res3 = matrix[2][0]*x1 + matrix[2][1]*x2 + matrix[2][2]*x3 - b[2];
        
        System.out.println("\nНевязка:");
        System.out.println("Для первого уравнения: " + res1);
        System.out.println("Для второго уравнения: " + res2);
        System.out.println("Для третьего уравнения: " + res3);
        
        // Сравнение суммы решений с erf(1.0)
        System.out.println("\nСумма решений: " + (x1 + x2 + x3));
        System.out.println("erf(1.0): " + erf(1.0));
    }
    
    public static void solveSecondSystem() {
        // Коэффициенты второй системы
        double[][] matrix = {
            {0.1, 0.2, 0.3},
            {0.4, 0.5, 0.6},
            {0.7, 0.8, 0.9}
        };
        
        double[] b = {0.1, 0.3, 0.5};
        
        double detA = determinant(matrix);
        
        if (Math.abs(detA) < 0.000001) {
            System.out.println("Система имеет бесконечно много решений");
            
            // Попробуем найти хотя бы одно решение
            // Упростим систему, исключив последнее уравнение
            double x3 = 1.0; // Просто предположим
            
            // Решаем первые два уравнения
            // 0.1x1 + 0.2x2 = 0.1 - 0.3x3
            // 0.4x1 + 0.5x2 = 0.3 - 0.6x3
            
            double right1 = 0.1 - 0.3*x3;
            double right2 = 0.3 - 0.6*x3;
            
            // Решаем систему 2x2
            double det = 0.1*0.5 - 0.2*0.4;
            double x1 = (right1*0.5 - 0.2*right2) / det;
            double x2 = (0.1*right2 - right1*0.4) / det;
            
            System.out.println("Одно из возможных решений:");
            System.out.println("x1 = " + x1);
            System.out.println("x2 = " + x2);
            System.out.println("x3 = " + x3);
        } else {
            System.out.println("Система имеет единственное решение");
            // Здесь должен быть код решения
        }
    }
    
    // Простая функция для вычисления определителя 3x3
    public static double determinant(double[][] matrix) {
        return matrix[0][0]*(matrix[1][1]*matrix[2][2] - matrix[1][2]*matrix[2][1])
             - matrix[0][1]*(matrix[1][0]*matrix[2][2] - matrix[1][2]*matrix[2][0])
             + matrix[0][2]*(matrix[1][0]*matrix[2][1] - matrix[1][1]*matrix[2][0]);
    }
    
    // Замена столбца в матрице
    public static double[][] replaceColumn(double[][] matrix, double[] column, int colIndex) {
        double[][] result = new double[3][3];
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                result[i][j] = matrix[i][j];
            }
            result[i][colIndex] = column[i];
        }
        return result;
    }
}
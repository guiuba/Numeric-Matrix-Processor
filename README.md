# Numeric-Matrix-Processor
Writing this program was a good exercise on how to deal with matrices, including addition, multiplication, finding the determinant and the inverse.
That is my implementation of Jet Brains Academy Numeric-Matrix-Processor project, stage 6/6.

Example of how it works:

(stage 6/6)

![image](https://user-images.githubusercontent.com/69851038/118030624-ee74b380-b33b-11eb-858e-a43e03e8dcde.png)

(stage 5/6)

![image](https://user-images.githubusercontent.com/69851038/118030884-3dbae400-b33c-11eb-8202-dd685f7949eb.png)

(stage 4/6)

![image](https://user-images.githubusercontent.com/69851038/118031228-b15cf100-b33c-11eb-9d16-8bb8e93f89e7.png)

-------------------------------------------------
My code:


import java.util.*;


public class Main {

    static Scanner scan = new Scanner(System.in);
    static boolean quit = false;
    public static void main(String[] args) { 
        while (!quit) {
            switch (showMenu()) {
                case 1:
                    addMatrices();
                    break;
                case 2:
                    multMatrixByConst();
                    break;
                case 3:
                    multMatrices();
                    break;
                case 4:
                    transposeMatrix();
                    break;
                case 5:
                    String result = String.valueOf(calculateDeterminant(matrixScanner()))
                            .replaceAll("\\.0", "");
                    System.out.println("The result is: \n" + result);
                    break;
                case 6:
                    getInverseMatrix();
                    break;
                case 0:
                    quit = true;
                    break;
                default:
                    break;
            }
        }
    }

    public static int showMenu() {
        System.out.println(
                "1. Add matrices\n" +
                        "2. Multiply matrix by a constant\n" +
                        "3. Multiply matrices\n" +
                        "4. Transpose matrix\n" +
                        "5. Calculate a determinant\n" +
                        "6. Inverse matrix\n" +
                        "0. Exit");
        System.out.print("Your choice: ");
        return scan.nextInt();
    }

    public static void getInverseMatrix() {

        double[][] matrix = matrixScanner();
        double matrixDet = calculateDeterminant(matrix);
        if (matrixDet == 0.0) {
            System.out.println("This matrix doesn't have an inverse.");
        } else {
            printMatrix(multMatrixByConstAlg(getMainDiagonal(getCoFactorMatrix(matrix)), 1 / matrixDet));
        }

    }

    public static double[][] getCoFactorMatrix(double[][] matrix) {
        int dim = matrix.length;
        double[][] coFactorMatrix = new double[dim][dim];

        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[i].length; j++) {
                coFactorMatrix[i][j] = Math.pow(-1, 2 + i + j) * calculateDeterminant(getMinor(matrix, i, j));
            }
        }
        return coFactorMatrix;
    }

    public static double calculateDeterminant(double[][] matrix) {
        double result = 0;
        if (matrix.length == 1) {
            result = matrix[0][0];

        } else if (matrix.length > 2) {
            for (int i = 0; i < matrix[0].length; i++) {
                result += matrix[0][i] * Math.pow(-1, i) * calculateDeterminant(getMinor(matrix, 0, i));
            }

        } else if (matrix.length == 2) {
            result = matrix[0][0] * matrix[1][1] - matrix[1][0] * matrix[0][1];
        }
        return result;
    }

    public static double[][] getMinor(double[][] matrix, int exc_row, int exc_col) {
        double[][] minor = new double[matrix.length - 1][matrix[0].length - 1];
        int i2 = 0;
        int j2 = 0;
        for (int i = 0; i < matrix.length; i++) {
            j2 = 0;
            if (i == exc_row) {
                continue;
            } else {
                for (int j = 0; j < matrix[0].length; j++) {
                    if (j == exc_col) {
                        continue;
                    } else {
                        minor[i2][j2] = matrix[i][j];
                        j2++;
                    }
                }
                i2++;
            }
        }
        return minor;
    }


    public static void printMatrix(double[][] matrix) {
        System.out.println("The result is:");
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[i].length; j++) {
                System.out.printf("%5.2f ", matrix[i][j]);
                //     System.out.print(matrix[i][j] + " ");
          //      String subResult1 = String.format("%5.2f ", matrix[i][j])
          //              .replaceAll(",", ".")
                        //      .replaceAll("\\.00", "")
            //            .replaceAll("-?0\\.00", "0");

                //     subResult.replaceAll("\\.00", "")
                //    Pattern pattern = Pattern.compile("\\d+\\.\\d0");
                //     Matcher matcher = pattern.matcher(subResult1);
                //      String pattern = "\\d*.\\d0";
                //    String result = matcher.matches() ? subResult1.endsWith("0")strip()trim()substring(0,subResult1.length() - 2) : subResult1;
                //       String result = subResult1.endsWith("\\\\d+\\\\.\\\\d0") ? subResult1.substring(0, subResult1.length() - 2) : subResult1;
                //       System.out.print(result + " ");
                //   System.out.printf("%.2f ", matrix[i][j]);
                //          System.out.printf("%.2f ", matrix[i][j]);
         //       System.out.printf("%s ", subResult1);
            }
            System.out.println();
        }
        System.out.println();
    }

    public static double[][] getMainDiagonal(double[][] matrix) {
        int dim = matrix.length;
        double[][] matrixAux = new double[dim][dim];
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[i].length; j++) {
                matrixAux[i][j] = matrix[j][i];
            }
        }
        return matrixAux;
    }

    public static double[][] getSideDiagonal(double[][] matrix) {
        int dim = matrix.length;
        double[][] matrixAux = new double[dim][dim];
        for (int i = dim - 1; i >= 0; i--) {
            for (int j = dim - 1; j >= 0; j--) {
                matrixAux[dim - 1 - i][dim - 1 - j] = matrix[j][i];
            }
        }
        return matrixAux;
    }

    public static double[][] getVerticalLine(double[][] matrix) {
        int dim = matrix.length;
        double[][] matrixAux = new double[dim][dim];
        for (int i = 0; i < dim; i++) {
            for (int j = dim - 1; j >= 0; j--) {
                matrixAux[i][dim - 1 - j] = matrix[i][j];
            }
        }
        return matrixAux;
    }

    public static double[][] getHorizontalLine(double[][] matrix) {
        int dim = matrix.length;
        double[][] matrixAux = new double[dim][dim];
        for (int i = matrix.length - 1; i >= 0; i--) {
            for (int j = 0; j < matrix[i].length; j++) {
                matrixAux[dim - 1 - i][j] = matrix[i][j];
            }
        }
        return matrixAux;
    }


    public static void transposeMatrix() {
        System.out.println(
                "1. Main diagonal\n" +
                        "2. Side diagonal\n" +
                        "3. Vertical line\n" +
                        "4. Horizontal line");
        System.out.print("Your choice: ");
        int choice = scan.nextInt();
        double[][] matrixToTranspose = matrixScanner();
        System.out.println("The result is:");
        switch (choice) {
            case 1:
                printMatrix(getMainDiagonal(matrixToTranspose));
                break;
            case 2:
                printMatrix(getSideDiagonal(matrixToTranspose));
                break;
            case 3:
                printMatrix(getVerticalLine(matrixToTranspose));
                break;
            case 4:
                printMatrix(getHorizontalLine(matrixToTranspose));
                break;
            default:
                break;
        }
    }

    public static void multMatrices() {
        System.out.println("Enter size of first matrix:");
        int aRows = scan.nextInt();
        int aCols = scan.nextInt();
        System.out.println("Enter first matrix:");
        double[][] matrixA = matrixScanner(aRows, aCols);

        System.out.println("Enter size of second matrix:");
        int bRows = scan.nextInt();
        int bCols = scan.nextInt();
        double[][] matrixB = matrixScanner(bRows, bCols);

        if (aCols != bRows) {
            System.out.println("ERROR");
        } else {
            double sum;
            System.out.println("The result is:");
            for (int i = 0; i < aRows; i++) {
                for (int j = 0; j < bCols; j++) {
                    sum = 0;
                    for (int k = 0; k < bRows; k++) {
                        sum += matrixA[i][k] * matrixB[k][j];
                    }
                    System.out.print(String.valueOf(String.format("%.2f", sum)).replaceAll(",00", "") + " ");
                }
                System.out.println();
            }
            System.out.println();
        }
    }

    public static double[][] multMatrixByConstAlg(double[][] matrix, double constant) {
        int dim = matrix.length;
        double[][] result = new double[dim][dim];
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix.length; j++) {
                result[i][j] = matrix[i][j] * constant;
            }
        }
        return result;
    }

    public static void multMatrixByConst() {
        double[][] matrix = matrixScanner();
        System.out.print("Enter constant: ");
        double constant = Double.parseDouble(scan.next());
        System.out.println("The result is:");
        printMatrix(multMatrixByConstAlg(matrix, constant));
    }


    public static double[][] matrixScanner(int rowsNum, int colsNum) {
        double[][] matrixAux = new double[rowsNum][colsNum];
        for (int i = 0; i < matrixAux.length; i++) {
            for (int j = 0; j < matrixAux[i].length; j++) {
                matrixAux[i][j] = Double.parseDouble(scan.next());
            }
        }
        return matrixAux;
    }


    public static double[][] matrixScanner() {
        System.out.println("Enter size of matrix:");
        int rows = scan.nextInt();
        int cols = scan.nextInt();
        double[][] matrix = new double[rows][cols];
        System.out.println("Enter matrix:");
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                matrix[i][j] = Double.parseDouble(scan.next());
            }
        }
        return matrix;
    }

    public static void addMatrices() {
        System.out.println("Enter size of first matrix:");
        int aRows = scan.nextInt();
        int aCols = scan.nextInt();
        System.out.println("Enter first matrix:");
        double[][] matrixA = matrixScanner(aRows, aCols);

        System.out.println("Enter size of second matrix:");
        int bRows = scan.nextInt();
        int bCols = scan.nextInt();
        double[][] matrixB;
        if (aRows != bRows || aCols != bCols) {
            System.out.println("ERROR");
        } else {
            System.out.println("Enter second matrix:");
            matrixB = matrixScanner(bRows, bCols);
            System.out.println("The result is:");


            for (int i = 0; i < matrixA.length; i++) {
                for (int j = 0; j < matrixA[i].length; j++) {
                    double aux = matrixA[i][j] + matrixB[i][j];
                    String s = String.format("%.2f", aux);
                    System.out.print(s.replaceAll(",00", "") + " ");
                }
                System.out.println();
            }
            System.out.println();
        }
    }
}


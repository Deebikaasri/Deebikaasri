package myapp;

import java.io.*;
import java.util.*;

public class Main {

    public static void main(String[] args) {
        String csvFile = "C:\\Users\\kamal\\Downloads\\HOUSE .csv"; // ✅ Replace with your actual path
        String line;
        String csvSplitBy = ",";
        
        List<Double> sqftList = new ArrayList<>();
        List<Double> bedroomsList = new ArrayList<>();
        List<Double> priceList = new ArrayList<>();

        try (BufferedReader br = new BufferedReader(new FileReader(csvFile))) {
            br.readLine(); // Skip header

            while ((line = br.readLine()) != null) {
                String[] row = line.split(csvSplitBy);
                try {
                    double bedrooms = Double.parseDouble(row[2]);
                    double sqft = Double.parseDouble(row[5]);
                    double price = Double.parseDouble(row[1]);

                    bedroomsList.add(bedrooms);
                    sqftList.add(sqft);
                    priceList.add(price);
                } catch (Exception e) {
                    // Skip bad rows
                }
            }

            int n = priceList.size();
            double sumX1 = 0, sumX2 = 0, sumY = 0;
            double sumX1Sq = 0, sumX2Sq = 0, sumX1X2 = 0;
            double sumX1Y = 0, sumX2Y = 0;

            for (int i = 0; i < n; i++) {
                double x1 = sqftList.get(i);
                double x2 = bedroomsList.get(i);
                double y = priceList.get(i);

                sumX1 += x1;
                sumX2 += x2;
                sumY += y;
                sumX1Sq += x1 * x1;
                sumX2Sq += x2 * x2;
                sumX1X2 += x1 * x2;
                sumX1Y += x1 * y;
                sumX2Y += x2 * y;
            }

            // Calculate coefficients using Normal Equation approximation
            double denominator = (sumX1Sq * sumX2Sq - sumX1X2 * sumX1X2);
            double b1 = (sumX2Sq * sumX1Y - sumX1X2 * sumX2Y) / denominator;
            double b2 = (sumX1Sq * sumX2Y - sumX1X2 * sumX1Y) / denominator;
            double a = (sumY - b1 * sumX1 - b2 * sumX2) / n;

            Scanner sc = new Scanner(System.in);
            System.out.print("Enter square footage: ");
            double sqftInput = sc.nextDouble();
            System.out.print("Enter number of bedrooms: ");
            double bedInput = sc.nextDouble();

            double predictedPrice = a + b1 * sqftInput + b2 * bedInput;
            System.out.printf("Predicted House Price: $%.2f\n", predictedPrice);

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

import org.json.JSONArray;
import org.json.JSONObject;
import java.io.*;
import java.net.HttpURLConnection;
import java.net.URL;

public class scrapeBenchling {
    public static void main(String[] args) {
        try {
            String apiUrl = "https://benchling.com/api/v2/feature-libraries?pageSize=50&sort=modifiedAt%3Adesc";
            String token = "eyJhbGciOiJFUzUxMiIsImJuY2giOjEsInR5cCI6IkpXVCJ9.eyJjbGllbnRfaWQiOiJibmNoLXN3YWdnZXItYXBpLWRvY3MiLCJleHAiOjE3MDIwODE2NjYsImlhdCI6MTcwMjA4MTM2NiwiYXVkIjoiYmVuY2hsaW5nLmNvbSIsImlzcyI6ImJlbmNobGluZy5jb20iLCJqdGkiOiIyLXhUUVJoVHl2ZkZwbGR4Q0dMby1nIiwic3ViIjoiZW50XzFXS3NyYVBmIiwicmwiOlsiMDozMDA6MzA6Y3QiXX0.ANvJcRD3PuKr9JEvtquTHnibDrL6qhRTu6SMaE1WCoHk6NRsBtXpYfNqldXCIVFmMFAw-wIvG_erjhdB_ol1HAykAPAyTP_M5cxo06-hzlTkedeoNpM1BZ6L92XCl_x4668bTtC7wTYMe0PbTtVsv5TqHWUpnI55qf3lIhY5bOCAPCif";

            URL url = new URL(apiUrl);
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            conn.setRequestMethod("GET");
            conn.setRequestProperty("Authorization", "Bearer " + token);
            conn.setRequestProperty("Accept", "application/json");

            int responseCode = conn.getResponseCode();
            System.out.println("Response Code : " + responseCode);

            if (responseCode == HttpURLConnection.HTTP_OK) {
                BufferedReader in = new BufferedReader(new InputStreamReader(conn.getInputStream()));
                String inputLine;
                StringBuffer response = new StringBuffer();

                while ((inputLine = in.readLine()) != null) {
                    response.append(inputLine);
                }
                in.close();

                // Write to a local file
                writeToFile(response.toString());
            } else {
                System.out.println("GET request not worked");
            }

        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private static void writeToFile(String jsonData) {
        try {
            FileWriter fileWriter = new FileWriter("featureLibraries.json");
            fileWriter.write(jsonData);
            fileWriter.close();
            System.out.println("Data written to featureLibraries.json");
        } catch (IOException e) {
            System.out.println("An error occurred while writing to the file.");
            e.printStackTrace();
        }
    }
}

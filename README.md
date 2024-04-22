# AI-DIGITAL-ASSIGNMENT
natural language processing
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Properties;
import org.apache.commons.text.StringEscapeUtils;
import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.impl.client.HttpClients;
import org.json.JSONObject;

public class SentimentAnalysisExample {
    public static void main(String[] args) throws IOException {
        // Read input text from the user
        BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
        System.out.print("Enter text to analyze sentiment: ");
        String inputText = reader.readLine();

        String cleanedText = StringEscapeUtils.escapeJava(inputText);

        JSONObject responseJson = performSentimentAnalysis(cleanedText);

  
        if (responseJson != null) {
            String polarity = responseJson.getString("polarity");
            String subjectivity = responseJson.getString("subjectivity");

            System.out.println("Sentiment Polarity: " + polarity);
            System.out.println("Sentiment Subjectivity: " + subjectivity);
        } else {
            System.out.println("Failed to analyze sentiment.");
        }
    }

    public static JSONObject performSentimentAnalysis(String text) throws IOException {
       
        HttpClient httpClient = HttpClients.createDefault();
        HttpPost httpPost = new HttpPost("https://textblobapi.com/api/sentiment/");

    
        Properties params = new Properties();
        params.setProperty("text", text);
        params.setProperty("token", "your_api_token"); // Replace "your_api_token" with your actual API token

     
        httpPost.setEntity(new HttpEntity(params.toString()));

    
        HttpResponse response = httpClient.execute(httpPost);
        BufferedReader reader = new BufferedReader(new InputStreamReader(response.getEntity().getContent()));
        StringBuilder responseContent = new StringBuilder();
        String line;
        while ((line = reader.readLine()) != null) {
            responseContent.append(line);
        }

     
        JSONObject responseJson = new JSONObject(responseContent.toString());
        return responseJson;
    }
}

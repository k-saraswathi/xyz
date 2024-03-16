import java.util.HashMap;
import java.util.Map;
import java.util.Random;

public class LinkShortener {
    private Map<String, String> shortToLongMap;
    private Map<String, String> longToShortMap;
    private Random random;
    private static final String CHARACTERS = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
    private static final int SHORT_URL_LENGTH = 6;

    public LinkShortener() {
        shortToLongMap = new HashMap<>();
        longToShortMap = new HashMap<>();
        random = new Random();
    }

    public String shortenURL(String longURL) {
        if (longToShortMap.containsKey(longURL)) {
            return longToShortMap.get(longURL);
        }

        String shortURL = generateShortURL();
        while (shortToLongMap.containsKey(shortURL)) {
            shortURL = generateShortURL();
        }

        shortToLongMap.put(shortURL, longURL);
        longToShortMap.put(longURL, shortURL);

        return shortURL;
    }

    public String expandURL(String shortURL) {
        return shortToLongMap.getOrDefault(shortURL, "Short URL not found.");
    }

    private String generateShortURL() {
        StringBuilder shortURL = new StringBuilder();
        for (int i = 0; i < SHORT_URL_LENGTH; i++) {
            int index = random.nextInt(CHARACTERS.length());
            shortURL.append(CHARACTERS.charAt(index));
        }
        return shortURL.toString();
    }

    public static void main(String[] args) {
        LinkShortener linkShortener = new LinkShortener();
        String longURL = "https://www.example.com";
        String shortURL = linkShortener.shortenURL(longURL);
        System.out.println("Short URL: " + shortURL);
        System.out.println("Expanded URL: " + linkShortener.expandURL(shortURL));
    }
}

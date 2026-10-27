
# Metro-
import java.util.*;

class Station {
    final String name;
    final String line;

    Station(String name, String line) {
        this.name = name;
        this.line = line;
    }
}

class Edge {
    final String to;
    final double km;
    final double minutes;
    final boolean transfer;

    Edge(String to, double km, double minutes, boolean transfer) {
        this.to = to;
        this.km = km;
        this.minutes = minutes;
        this.transfer = transfer;
    }
}

class MetroGraph {
    final HashMap<String, Station> stations = new HashMap<>();
    final HashMap<String, LinkedList<Edge>> graph = new HashMap<>();
    final HashMap<String, List<String>> lineOrders = new HashMap<>();

    static String normalize(String s) {
        String v = s.trim().toLowerCase(Locale.ROOT)
                .replaceAll("[^a-z0-9]", "");

        if (v.equals("whitefield") || v.equals("kadugodi")
                || v.equals("whitefieldkadugodi"))
            return "whitefieldkadugodi";

        if (v.equals("majestik")
                || v.equals("nadaprabhukempegowdastation")
                || v.equals("nadaprabhukempegowdastationmajestic"))
            return "majestic";

        if (v.equals("rvroad")
                || v.equals("rashtriyavidyalayaroad")
                || v.equals("rashtreeyavidyalayaroad"))
            return "rvroad";

        if (v.equals("centralcollege")
                || v.equals("sirmvisvesvarayastationcentralcollege"))
            return "centralcollege";

        if (v.equals("madavara") || v.equals("biec")
                || v.equals("madavarabiec"))
            return "madavarabiec";

        if (v.equals("krmarket") || v.equals("krishnarajendramarket"))
            return "krmarket";

        if (v.equals("jpnagar") || v.equals("jayaprakashnagar"))
            return "jpnagar";

        if (v.equals("pantharapalayanayandahalli")
                || v.equals("nayandahalli"))
            return "pantharapalayanayandahalli";

        if (v.equals("mysoreroad") || v.equals("mysururoad"))
            return "mysoreroad";

        if (v.equals("hoodijunction"))
            return "hoodi";

        if (v.equals("krpuram") || v.equals("krishnarajapura"))
            return "krpuram";

        if (v.equals("baiyappanahalli") || v.equals("bayappanahalli"))
            return "baiyappanahalli";

        if (v.equals("yeshwanthpur") || v.equals("yeshwantpur"))
            return "yeshwanthpur";

        if (v.equals("silkinstitute"))
            return "silkinstitute";

        return v;
    }

    String id(String name, String line) {
        return line + "|" + normalize(name);
    }

    void edge(String a, String b, double km,
              double min, boolean transfer) {
        graph.get(a).add(new Edge(b, km, min, transfer));
    }

    void addLine(String line, String[] names) {
        lineOrders.put(line, Arrays.asList(names));

        for (String name : names) {
            String key = id(name, line);
            stations.put(key, new Station(name, line));
            graph.putIfAbsent(key, new LinkedList<>());
        }

        for (int i = 0; i < names.length - 1; i++) {
            String a = id(names[i], line);
            String b = id(names[i + 1], line);

            edge(a, b, 1.2, 2.5, false);
            edge(b, a, 1.2, 2.5, false);
        }
    }

    void connect(String name, String line1, String line2) {
        String a = id(name, line1);
        String b = id(name, line2);

        edge(a, b, 0, 4, true);
        edge(b, a, 0, 4, true);
    }

    void createNetwork() {
        String[] purple = {
            "Whitefield (Kadugodi)",
            "Hopefarm Channasandra",
            "Kadugodi Tree Park",
            "Pattandur Agrahara",
            "Sri Sathya Sai Hospital",
            "Nallurahalli",
            "Kundalahalli",
            "Seetharampalya",
            "Hoodi",
            "Garudacharpalya",
            "Singayyanapalya",
            "KR Puram",
            "Benniganahalli",
            "Baiyappanahalli",
            "Swami Vivekananda Road",
            "Indiranagar",
            "Halasuru",
            "Trinity",
            "MG Road",
            "Cubbon Park",
            "Vidhana Soudha",
            "Central College",
            "Majestic",
            "City Railway Station (KSR Bengaluru)",
            "Magadi Road",
            "Hosahalli",
            "Vijayanagar",
            "Attiguppe",
            "Deepanjali Nagar",
            "Mysore Road",
            "Pantharapalya-Nayandahalli",
            "Rajarajeshwari Nagar",
            "Jnanabharathi",
            "Pattanagere",
            "Kengeri Bus Terminal",
            "Kengeri",
            "Challaghatta"
        };

        String[] green = {
            "Madavara (BIEC)",
            "Chikkabidarakallu",
            "Manjunathanagar",
            "Nagasandra",
            "Dasarahalli",
            "Jalahalli",
            "Peenya Industry",
            "Peenya",
            "Goraguntepalya",
            "Yeshwanthpur",
            "Sandal Soap Factory",
            "Mahalakshmi",
            "Rajajinagar",
            "Kuvempu Road",
            "Srirampura",
            "Mantri Square Sampige Road",
            "Majestic",
            "Chickpete",
            "KR Market",
            "National College",
            "Lalbagh",
            "South End Circle",
            "Jayanagar",
            "RV Road",
            "Banashankari",
            "JP Nagar",
            "Yelachenahalli",
            "Konanakunte Cross",
            "Doddakallasandra",
            "Vajarahalli",
            "Thalaghattapura",
            "Silk Institute"
        };

        String[] yellow = {
            "RV Road",
            "Ragigudda",
            "Jayadeva Hospital",
            "BTM Layout",
            "Central Silk Board",
            "Bommanahalli",
            "Hongasandra",
            "Kudlu Gate",
            "Singasandra",
            "Hosa Road",
            "Beratena Agrahara",
            "Electronic City",
            "Infosys Foundation Konappana Agrahara",
            "Huskur Road",
            "Hebbagodi",
            "Bommasandra"
        };

        addLine("Purple", purple);
        addLine("Green", green);
        addLine("Yellow", yellow);

        connect("Majestic", "Purple", "Green");
        connect("RV Road", "Green", "Yellow");
    }

    ArrayList<String> matches(String name) {
        String wanted = normalize(name);
        ArrayList<String> found = new ArrayList<>();

        for (Map.Entry<String, Station> e : stations.entrySet()) {
            if (normalize(e.getValue().name).equals(wanted)) {
                found.add(e.getKey());
            }
        }

        Collections.sort(found);
        return found;
    }
}

class PathFinder {
    final MetroGraph metro;

    PathFinder(MetroGraph metro) {
        this.metro = metro;
    }

    ArrayList<String> findRoute(String source, String destination) {
        ArrayList<String> starts = metro.matches(source);
        ArrayList<String> ends = metro.matches(destination);

        if (starts.isEmpty()) {
            throw new IllegalArgumentException(
                    "Source station not found: " + source);
        }

        if (ends.isEmpty()) {
            throw new IllegalArgumentException(
                    "Destination station not found: " + destination);
        }

        HashSet<String> endSet = new HashSet<>(ends);
        HashSet<String> visited = new HashSet<>();
        Queue<String> queue = new LinkedList<>();
        HashMap<String, String> parent = new HashMap<>();

        for (String start : starts) {
            queue.add(start);
            visited.add(start);
            parent.put(start, null);
        }

        String found = null;

        while (!queue.isEmpty()) {
            String current = queue.poll();

            if (endSet.contains(current)) {
                found = current;
                break;
            }

            for (Edge e : metro.graph.get(current)) {
                if (visited.add(e.to)) {
                    parent.put(e.to, current);
                    queue.add(e.to);
                }
            }
        }

        if (found == null) {
            throw new IllegalArgumentException("No connected route found.");
        }

        ArrayList<String> route = new ArrayList<>();

        for (String at = found; at != null; at = parent.get(at)) {
            route.add(at);
        }

        Collections.reverse(route);
        return route;
    }
}

class ItineraryFormatter {
    final MetroGraph metro;

    ItineraryFormatter(MetroGraph metro) {
        this.metro = metro;
    }

    static int calculateFare(double distance) {
        if (distance <= 2) return 10;
        else if (distance <= 4) return 20;
        else if (distance <= 6) return 30;
        else if (distance <= 8) return 40;
        else if (distance <= 10) return 50;
        else if (distance <= 15) return 60;
        else if (distance <= 20) return 70;
        else if (distance <= 25) return 80;
        else return 90;
    }

    void print(ArrayList<String> route) {
        double km = 0;
        double minutes = 0;
        int transfers = 0;

        ArrayList<String> names = new ArrayList<>();

        for (int i = 0; i < route.size(); i++) {
            Station s = metro.stations.get(route.get(i));
            names.add(s.name);

            if (i == 0) continue;

            String previousId = route.get(i - 1);

            for (Edge e : metro.graph.get(previousId)) {
                if (e.to.equals(route.get(i))) {
                    km += e.km;
                    minutes += e.minutes;

                    if (e.transfer) {
                        transfers++;
                    }

                    break;
                }
            }
        }

        // Remove duplicate names caused by changing lines.
        ArrayList<String> unique = new ArrayList<>();

        for (String n : names) {
            if (unique.isEmpty()
                    || !unique.get(unique.size() - 1).equals(n)) {
                unique.add(n);
            }
        }

        StringBuilder routeText = new StringBuilder();

        for (int i = 0; i < unique.size(); i++) {
            if (i > 0) routeText.append(" -> ");
            routeText.append(unique.get(i));
        }

        int fare = calculateFare(km);

        System.out.println("\n========== METRO JOURNEY ==========");
        System.out.println("Source      : " + unique.get(0));
        System.out.println("Destination : "
                + unique.get(unique.size() - 1));

        System.out.println("\nComplete route:");
        System.out.println(routeText);

        // Interchange and next-metro direction are shown together.
        System.out.println("\nInterchanges (" + transfers + "):");

        boolean interchangeFound = false;

        for (int i = 0; i < route.size() - 1; i++) {
            Station current = metro.stations.get(route.get(i));
            Station next = metro.stations.get(route.get(i + 1));

            if (!current.line.equals(next.line)) {
                interchangeFound = true;

                System.out.println("\n" + current.name + " ("
                        + current.line + " -> " + next.line + " Line)");

                // Find the direction using the next station on the new line.
                if (i + 2 < route.size()) {
                    Station following =
                            metro.stations.get(route.get(i + 2));

                    List<String> order =
                            metro.lineOrders.get(next.line);

                    int interchangeIndex = order.indexOf(next.name);
                    int followingIndex = order.indexOf(following.name);

                    if (following.line.equals(next.line)
                            && interchangeIndex >= 0
                            && followingIndex >= 0) {

                        String direction;

                        if (followingIndex > interchangeIndex) {
                            direction = order.get(order.size() - 1);
                        } else {
                            direction = order.get(0);
                        }

                        System.out.println("Next metro: "
                                + next.line + " Line towards " + direction);

                    } else {
                        System.out.println(
                                "Follow the route to your destination.");
                    }
                } else {
                    System.out.println(
                            "You have reached your destination.");
                }
            }
        }

        if (!interchangeFound) {
            System.out.println("No interchange required.");
        }

        System.out.println("Station-to-station segments: "
                + Math.max(0, route.size() - 1));

        System.out.printf("Estimated distance: %.1f km%n", km);
        System.out.printf("Estimated time    : %.0f minutes%n", minutes);
        System.out.println("Estimated fare    : Rs. " + fare);

        System.out.println("\nNOTE: Distance, time and fare are project estimates.");
        System.out.println("They may differ from the actual Namma Metro ticket price.");
        System.out.println("===================================");
    }
}

public class Main {
    private static final Set<String> FUTURE_STATIONS =
            new HashSet<>(Arrays.asList(
                    "devanahalli",
                    "kempegowda international airport",
                    "airport",
                    "nagawara",
                    "kalena agrahara",
                    "hebbal",
                    "marathahalli",
                    "bellandur",
                    "kadabagere",
                    "kogilu cross",
                    "jakkur"
            ));

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        MetroGraph metro = new MetroGraph();
        metro.createNetwork();

        System.out.println("===== BENGALURU METRO ROUTE FINDER =====");
        System.out.println("Lines included: Purple, Green and Yellow");

        System.out.print("Enter source station: ");
        String source = sc.nextLine();

        System.out.print("Enter destination station: ");
        String destination = sc.nextLine();

        try {
            ArrayList<String> route =
                    new PathFinder(metro).findRoute(source, destination);

            new ItineraryFormatter(metro).print(route);

        } catch (IllegalArgumentException e) {
            System.out.println("\nNo route found.");
            System.out.println(e.getMessage());

            String src = source.trim().toLowerCase(Locale.ROOT);
            String dst = destination.trim().toLowerCase(Locale.ROOT);

            if (FUTURE_STATIONS.contains(src)
                    || FUTURE_STATIONS.contains(dst)) {
                System.out.println(
                        "That station may belong to a planned or under-construction corridor.");
            } else {
                System.out.println(
                        "The station is not in this program's station list, or its name may be incorrect.");
                System.out.println(
                        "Check the spelling or use the station name listed in the program.");
            }

        } finally {
            sc.close();
        }
    }
}
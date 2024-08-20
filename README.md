# 20-08-2024-Hackathon

import java.util.ArrayList;
import java.util.Scanner;

class AuctionItem {
    private String itemName;
    private String description;
    private double startingPrice;
    private double currentBid;
    private String highestBidder;
    private boolean isOpen;

    public AuctionItem(String itemName, String description, double startingPrice) {
        this.itemName = itemName;
        this.description = description;
        this.startingPrice = startingPrice;
        this.currentBid = startingPrice;
        this.highestBidder = "None";
        this.isOpen = true;
    }

    public String getItemName() {
        return itemName;
    }

    public double getCurrentBid() {
        return currentBid;
    }

    public boolean isOpen() {
        return isOpen;
    }

    public void placeBid(String bidderName, double bidAmount) {
        if (isOpen && bidAmount > currentBid) {
            currentBid = bidAmount;
            highestBidder = bidderName;
            System.out.println("Bid placed successfully by " + bidderName + " for " + itemName + " with amount $" + bidAmount);
        } else {
            System.out.println("Bid amount must be higher than the current bid or the auction is closed.");
        }
    }

    public void closeAuction() {
        if (isOpen) {
            isOpen = false;
            System.out.println("Auction closed for " + itemName + ". Winning bid: $" + currentBid + " by " + highestBidder);
        } else {
            System.out.println("Auction is already closed.");
        }
    }

    public void displayItemDetails() {
        System.out.println("Item Name: " + itemName);
        System.out.println("Description: " + description);
        System.out.println("Starting Price: $" + startingPrice);
        System.out.println("Current Bid: $" + currentBid);
        System.out.println("Highest Bidder: " + highestBidder);
        System.out.println("Status: " + (isOpen ? "Open" : "Closed"));
    }
}

class AuctionSystem {
    private ArrayList<AuctionItem> auctionItems = new ArrayList<>();
    private Scanner scanner = new Scanner(System.in);

    public void start() {
        System.out.println("Welcome to the Online Auction System");
        while (true) {
            System.out.println("\n1. Auctioneer: Add an Item");
            System.out.println("2. Bidder: Place a Bid");
            System.out.println("3. Auctioneer: Close Auction");
            System.out.println("4. View All Auctions");
            System.out.println("5. Exit");
            System.out.print("Choose an option: ");
            int choice = scanner.nextInt();
            scanner.nextLine(); // Consume newline

            switch (choice) {
                case 1:
                    addItem();
                    break;
                case 2:
                    placeBid();
                    break;
                case 3:
                    closeAuction();
                    break;
                case 4:
                    viewAllAuctions();
                    break;
                case 5:
                    System.out.println("Exiting system. Thank you!");
                    return;
                default:
                    System.out.println("Invalid choice, please try again.");
            }
        }
    }

    private void addItem() {
        System.out.print("Enter item name: ");
        String itemName = scanner.nextLine();
        System.out.print("Enter item description: ");
        String description = scanner.nextLine();
        System.out.print("Enter starting price: ");
        double startingPrice = scanner.nextDouble();
        scanner.nextLine(); 

        AuctionItem newItem = new AuctionItem(itemName, description, startingPrice);
        auctionItems.add(newItem);
        System.out.println("Item added successfully!");
    }

    private void placeBid() {
        System.out.print("Enter your name: ");
        String bidderName = scanner.nextLine();
        System.out.print("Enter the item name you want to bid on: ");
        String itemName = scanner.nextLine();
        AuctionItem item = findItemByName(itemName);

        if (item != null) {
            System.out.print("Enter your bid amount: ");
            double bidAmount = scanner.nextDouble();
            scanner.nextLine(); 
            item.placeBid(bidderName, bidAmount);
        } else {
            System.out.println("Item not found.");
        }
    }

    private void closeAuction() {
        System.out.print("Enter the item name you want to close auction for: ");
        String itemName = scanner.nextLine();
        AuctionItem item = findItemByName(itemName);

        if (item != null) {
            item.closeAuction();
        } else {
            System.out.println("Item not found.");
        }
    }

    private void viewAllAuctions() {
        if (auctionItems.isEmpty()) {
            System.out.println("No auctions available.");
        } else {
            for (AuctionItem item : auctionItems) {
                item.displayItemDetails();
                System.out.println();
            }
        }
    }

    private AuctionItem findItemByName(String itemName) {
        for (AuctionItem item : auctionItems) {
            if (item.getItemName().equalsIgnoreCase(itemName)) {
                return item;
            }
        }
        return null;
    }
}

public class OnlineAuctionSystem {
    public static void main(String[] args) {
        AuctionSystem auctionSystem = new AuctionSystem();
        auctionSystem.start();
    }
}


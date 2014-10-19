BlackJack


//Coding the game blackjack as a side project. Currently only has one player and a dealer, looking to implement more things into the game. 


package blackjack;

import java.text.DecimalFormat;
import java.util.Scanner;
import java.util.ArrayList;


public class BlackJack_Main
{
	
	public static String getCardName(int suit, int value)
	{
		String temp = "";
		
		switch(value)
		{
			case 1: temp = "Ace of "; break;
			case 2: temp = "Two of "; break;	
			case 3: temp = "Three of "; break;
			case 4: temp = "Four of "; break;
			case 5: temp = "Five of "; break;
			case 6: temp = "Six of "; break;
			case 7:	temp = "Seven of ";	break;
			case 8: temp = "Eight of "; break;
			case 9: temp = "Nine of "; break;
			case 10: temp = "Ten of "; break;
			case 11: temp = "Jack of "; break;	
			case 12: temp = "Queen of "; break;
			case 13: temp = "King of "; break;
		}
		
		switch(suit)
		{
			case 1: temp += "Hearts"; break;	
			case 2: temp += "Diamonds"; break;	
			case 3: temp += "Spades"; break;	
			case 4: temp += "Clubs"; break;
		}
		return temp;
	}
	
	public static int getHandSum(ArrayList<Integer> hand)
		{
			int sum = 0;
			for (int i = 0; i < hand.size(); i++)
				sum += hand.get(i);
			return sum;
		}
	
	public static void main(String[] args)
	{
		
				//Declare scanner object
				Scanner input = new Scanner (System.in);
				
				//Declare decimalFormat object
				DecimalFormat twoDig = new DecimalFormat("0.00");
				
				int suit, value = 0;											//Initializing everything to 0
				boolean running = true;											//Checks if game is still running
				boolean rerun = true;											//Re run the game
				char answer = 'Y';												//Input for user rerun
				ArrayList<Integer> hand = new ArrayList<Integer>();				//cards in current hand
				double money = 1000;											//Money player starts off with
				double bet; 													//Money player bets
				
			while(rerun)
			{
				
				rerun = false;
				
				//Setup the deck
				int[][] deck = new int [5][14]; 
				for (int i = 1; i < deck.length; i++)
					for (int j = 1; j < deck[0].length; j++)
					{
						deck[i][j] = 0;
					}

			
				//Begin the game
				System.out.println("Welcome to the BlackJack table. All aces are worth 11. Jacks, Queens, and Kings are worth 10. \nLet's Begin!");
				System.out.println("Player One you have $" + twoDig.format(money));
				
				//Betting
				System.out.println("How much would you like to bet?");
				bet = input.nextDouble();
				input.nextLine();
				
				while (bet > money || bet < 0)
				{
					System.out.println("Error. Please enter a valid number");
					bet = input.nextDouble();input.nextLine();
				}
				
				//Generate a card
				suit = (int)(Math.random() * 4 + 1);
				value = (int)(Math.random() * 13 + 1);
				
				//Check if first card is available
				if (deck[suit][value] == 0)
					deck[suit][value] = 1;
				else 
				{
					while (deck[suit][value] == 1)
					{
						suit = (int)(Math.random() * 4 + 1);
						value = (int)(Math.random() * 13 + 1);
					}
					deck[suit][value] = 1;
				}
				
				//Display first card
				System.out.print("Player One your cards are: ");
				System.out.print(getCardName(suit,value));
				
				//Adding the first card value to the hand
				if (value >= 10)
					hand.add(10);
				else if (value == 1)
					hand.add(11);
				else 
					hand.add(value);
				
				//Generate another card
				suit = (int)(Math.random() * 4 + 1);
				value = (int)(Math.random() * 13 + 1);
				
				//Check if second card is available
				if (deck[suit][value] == 0)
					deck[suit][value] = 1;
				else 
				{
					while (deck[suit][value] == 1)
					{
						suit = (int)(Math.random() * 4 + 1);
						value = (int)(Math.random() * 13 + 1);
					}
					deck[suit][value] = 1;
				}
				
				
				//Display the second card
				System.out.print(" and ");
				System.out.print(getCardName(suit,value));

				
				//Adding the second card value to the hand
				if (value >= 10)
					hand.add(10);
				else if (value == 1)
					hand.add(11);
				else 
					hand.add(value);
			
			while(running)
			{
			
				//Checking the cards for a win or loss
				if (getHandSum(hand) == 21)
					{
						System.out.println("\nCongratulations you got BlackJack!");
						money += bet;
						System.out.println("You now have " + twoDig.format(money));
						running = false;
					}
				else if (getHandSum(hand) > 21)
					{
						money -= bet;
						System.out.println("\nSorry play again! Your hand is greater than 21");
						System.out.println("\nYou now have" + twoDig.format(money));
						running = false;
					}	
				
				else
					{
						System.out.println("\nWould you like to hit or stand? (H or S) ");
						char decision2 = input.nextLine().toUpperCase().charAt(0);
			
						if (decision2 == 'S')
							{
								System.out.println("Player one chooses to stand, play again.");
								running = false;
							}
						else if (decision2 == 'H')
						{	
								System.out.println("Player one your card is: ");
								suit = (int)(Math.random() * 4 + 1);
								value = (int)(Math.random() * 13 + 1);
								hand.add(value);
								System.out.print(getCardName(suit,value));
							
							if (getHandSum(hand) == 21)
								{
									System.out.println("\nCongratulations you got BlackJack!");
									System.out.println("\nYou now have $" + twoDig.format(money) + "!");
									running = false;
								}
							else if (getHandSum(hand) > 21)
								{
									System.out.print("\nSorry play again! The sum of your cards is greater than 21");
									money -= bet;
									System.out.println("\nYou now have $" + twoDig.format(money) + "");
									running = false;
								}	
							else if (getHandSum(hand) < 21)
								{
									System.out.println("\nThe sum of your cards is less than 21, would you like to hit or stand?");
								}	
						}
						// Replaying the game
						if (running == false)
						{
							System.out.println("\nWould you like to play again? (Y or N) ");
							answer = input.nextLine().toUpperCase().charAt(0);
							{
							if (answer == 'Y')
								rerun = true;
							else if (answer == 'N')
								running = false;	
							}
						}
					}
				}
			}
		}
	}



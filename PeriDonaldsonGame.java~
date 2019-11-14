import java.awt.event.KeyEvent;
import java.util.Random;
import java.util.*;
import java.awt.*;
import java.awt.event.*;
import java.awt.image.*;
import java.io.*;
import java.net.*;
import javax.imageio.*;
import javax.swing.*;

public class PeriDonaldsonGame {
	

	// Game window should be wider than tall:  H_DIM < W_DIM   
	// (more effectively using space)
	// # of cells vertically by default: height of game
	private static final int H_DIM = 5; 
	// # of cells horizontally by default: width of game
	private static final int W_DIM = 10;  
	// default location of the user at the start of the game
	private static final int U_ROW = 0;
	
	private static final int FACTOR = 3;      // you might change that this
	                           // setting or declaration when working on timing
	                           // (speed up and down the game)
	                           
	                           
	private final String USER_IMG = "kismet.png";  //ADD others for Avoid/Get items
	private final String AVOID_IMG = "sun.gif";
	private final String GET_IMG = "cricket.gif";
	private final String SPLASH = "splash.png";
	private final String BACKGROUND = "grass.gif";
	private final String LIFES = "mealworms.png";
	private final String LOSE = "lose.png";
	private final String WIN = "win.png";
	                        
	private static Random rand = new Random();  //USE ME 
	                                          // don't instantiate me every frame
	
	private GameGrid grid; // graphical window to display the game (the VIEW)
	                       // a 2D game made of two dimensional array of Cells
	private int userRow;
	private int userCol;
	
	private long msElapsed;		// game clock: number of ms since start of
	                          // game main loop---see play() method
	private int timerClicks;	// used to space animation and key presses
	private int timerDelay;		// to control speed of game
	private ArrayList<int[]> intlist = new ArrayList<int[]>();
	private int[] arrH = new int[H_DIM];
	private int[] addR = new int[H_DIM];
	private int score =0; //users score
	private int count = 1;
	private int clickCount = 6;// for the sunblockers count
	private boolean lost;
	
	public void makeIntList()
	{
		for(int i = 0; i < W_DIM; i++)
		{
			intlist.add(arrH);
		}
	}
	
	public PeriDonaldsonGame() {
		this(H_DIM, W_DIM, U_ROW);
	}
	
	public PeriDonaldsonGame(int hdim, int wdim, int urow) {
		
		init(hdim, wdim, urow);
	}
	
	private void init(int hdim, int wdim, int urow) {  
		
		/* initialize the game window

				NOTE: look at the various constructors to see what you can do!
				For example:
					grid = new GameGrid(hdim, wdim, Color.MAGENTA);
					
				You need to use the one that takes an image to implement the 
				splashscreen functionality (don't start there, but do it as your
				first extension)
		*/
		grid = new GameGrid(hdim, wdim);   
		
		/* initialize other aspects of game state */

		// animation settings
		timerClicks = 0;
		msElapsed = 0;
		timerDelay = 100;

		// store and initialize user position
		userRow = urow;
		grid.setCellImage(new Location(userRow, 0), USER_IMG);
		grid.setGameBackground(BACKGROUND);
		updateTitle();
		
	}
	
	public void play() {
		
		/* main game loop */
		splashScreen(SPLASH); //makes splash screen before game starts
		grid.setGameBackground(BACKGROUND);
		while (!isGameOver()) {
			
			grid.sleep(timerDelay); 	 // pause for some time (smooth animation)
			msElapsed += timerDelay;	 // count the total amount of time elapsed
			timerClicks++;				 // increment the timer count
		
			handleKeyPress();        // update state based on user key press
			
			handleMouseClick();      // when the game is running: 
			                         // click & read the console output 
			
			
			if (timerClicks % FACTOR == 0) {  // if it's the FACTOR timer tick
				                            // constant 3 initially
				scrollLeft();
				populateRightEdge();
			}
			
			updateTitle();
			msElapsed += timerDelay;
			
		}
		if(lost)
			splashScreen(LOSE);//loser ending screen
		else
			splashScreen(WIN);//winnning ending screen
		System.exit(0); // exit on click
		
		
	}
	
	public void handleMouseClick() {
		
		Location loc = grid.checkLastLocationClicked();
		/*System.out.println(loc);
		int test = loc.getCol();
		System.out.println(test);
		System.out.println(intlist.get(loc.getCol()));
		*/
		if (loc != null && grid.getCellImage(loc) == AVOID_IMG && clickCount!= 0) //makes sure that you cant click more suns then 6
		{
			int[] firstcol = intlist.get(loc.getCol()); //gets the column clicked on
			grid.setCellImage(new Location(loc.getRow(), loc.getCol()), null);
			firstcol[loc.getRow()] = 1;
			System.out.println("no more sun in location " + loc);
			clickCount--;
		}
	
	}

	
	public void handleKeyPress() {
			
		int key = grid.checkLastKeyPressed();
		
		//use Java constant names for key presses
		//http://docs.oracle.com/javase/7/docs/api/constant-values.html#java.awt.event.KeyEvent.VK_DOWN
		
		// Q for quit
		if (key == KeyEvent.VK_Q)
			System.exit(0);
		
		/* To help you with step 9: 
		   use the 'T' key to help you with implementing speed up/slow down/pause
    	   this prints out a debugging message */
		else if (key == KeyEvent.VK_T)  {
			boolean interval =  (timerClicks % FACTOR == 0);
			System.out.println("timerDelay " + timerDelay + " msElapsed reset " + 
				msElapsed + " interval " + interval);
		}
		
		else if(key == KeyEvent.VK_DOWN)//moves user down
		{
			if(userRow < (H_DIM-1))
			{
				grid.setCellImage(new Location(userRow, userCol), null);
				userRow++;
				grid.setCellImage(new Location(userRow, userCol), USER_IMG);
			}
		}
		else if(key == KeyEvent.VK_UP)//moves user up
		{
			if(userRow > 0)
			{
				grid.setCellImage(new Location(userRow, userCol), null);
				userRow--;
				grid.setCellImage(new Location(userRow, userCol), USER_IMG);
			}
		}
		else if(key == KeyEvent.VK_RIGHT)//moves user right
		{
			if(userCol < (W_DIM-1))
			{
				grid.setCellImage(new Location(userRow, userCol), null);
				userCol++;
				grid.setCellImage(new Location(userRow, userCol), USER_IMG);
			}
		}
		else if(key == KeyEvent.VK_LEFT)//moves user left
		{
			if(userCol > 0)
			{
				grid.setCellImage(new Location(userRow, userCol), null);
				userCol--;
				grid.setCellImage(new Location(userRow, userCol), USER_IMG);
			}
		}
		else if(key == KeyEvent.VK_COMMA)//slows down speed of game
		{
			timerDelay += 30;
		}
		else if(key == KeyEvent.VK_PERIOD)//speeds up the game
		{
			timerDelay -= 30;
		}
		else if(key == KeyEvent.VK_P)//pauses and unpauses the game
		{
			boolean paused = true;
			System.out.println("pause");
			while(paused == true)
			{
				GameGrid.sleep(200);
				int again = grid.checkLastKeyPressed();
				if(again == KeyEvent.VK_P)
				{
					paused = false;
				}
			}
		}
		else if(key == KeyEvent.VK_D)//pauses game and shows grid lines
		{
			boolean on = true;
			while(on == true)
			{
				grid.setLineColor(Color.BLUE);
				int again = grid.checkLastKeyPressed();
				if(again == KeyEvent.VK_D)
				{
					grid.setLineColor(null);
					on = false;
				}
			}
		}
		else if(key == KeyEvent.VK_S) //takes screen shot of the game
		{
			
			grid.save("screenshot" +count+ ".jpg");
			System.out.println("screenshot" +count+ ".jpg was saved");
			count++;
		}
	}
	
	// update game state to reflect adding in new cells in the right-most column 
	public void populateRightEdge() {
		
		int[] addR = new int[H_DIM]; //the right hand column to be made
		
		for(int i = 0; i < H_DIM; i++)
		{
			int num = rand.nextInt(H_DIM);// randomly decides what is going to go into the cell
			if(num == 0 && !(userRow == i && userCol == W_DIM -1))
			{
				grid.setCellImage(new Location(i, (W_DIM-1)), GET_IMG);//makes cell have a cricket in it
				addR[i] = 0;
			}
			else if(num > 2 && !(userRow == i && userCol == W_DIM -1))
			{
				grid.setCellImage(new Location(i, (W_DIM-1)), AVOID_IMG);//makes cell have a sun in it
				addR[i] = 2;
			}
			else if(num == 1 && !(userRow == i && userCol == W_DIM -1))
			{
				grid.setCellImage(new Location(i, (W_DIM-1)), null);//makes cell empty
				addR[i] = 1;
			}
			else if(num == 2 && !(userRow == i && userCol == W_DIM -1))
			{
				int num2 = rand.nextInt(H_DIM);
				if(num2 == 0)
				{
					grid.setCellImage(new Location(i, (W_DIM-1)), LIFES);//makes cell have mealworms in it
					addR[i] = 3;
				}
			}
			
		}
		intlist.set(W_DIM-1, addR);
		
	}
	
	// updates the game state to reflect scrolling left by one column 
	public void scrollLeft() {
		
		intlist.remove(0);
		intlist.add(addR);
		for(int i = 0; i < W_DIM -1; i++)
		{
			int[] place = intlist.get(i);
			for(int p = 0; p < H_DIM; p++)
			{
				if(place[p] == 0)
				{
					if(!(userRow == p && userCol == i)){
						grid.setCellImage(new Location(p, i), GET_IMG);
					}
					else{
						handleCollision(new Location(userRow,userCol)); // if the user is in the same spot
					}
				}
				else if(place[p] == 1 && !(userRow == p && userCol == i))
				{
					grid.setCellImage(new Location(p, i), null);
				}
				else if(place[p] == 2 )
				{
					if(!(userRow == p && userCol == i)){
						grid.setCellImage(new Location(p, i), AVOID_IMG);
					}
					else{
						handleCollision(new Location(userRow,userCol));// if the user is in the same spot
					}
				}
				else if(place[p] == 3 )
				{
					if(!(userRow == p && userCol == i)){
						grid.setCellImage(new Location(p, i), LIFES);
					}
					else{
						handleCollision(new Location(userRow,userCol));// if the user is in the same spot
					}
				}
			}
		}
	}
	
	public void handleCollision(Location loc) {
		int[] firstcol = intlist.get(userCol);
		if(firstcol[userRow]==0)// handles collision with crickets
		{
			score+= 5;
			firstcol[userRow] = 1;
			System.out.println("yummy cricket");
		}
		else if(firstcol[userRow]== 2)// handles collision with the sun
		{
			score-=5;
			firstcol[userRow] = 1;
			System.out.println("Nooooo not the sun");
		}
		else if(firstcol[userRow]== 3)// handles collision with mealworms
		{
			if(score < 0)
			{
				score = 0;
			}
			else
			{
				score+= 10;
			}
			firstcol[userRow] = 1;
			System.out.println("MEALWORMS MY FAVORITE");
		}
		
		updateTitle();
	}
	
	// return the "score" of the game 
	public int getScore() {
		return this.score;    
	}
	
	
	// update the title bar of the game window 
	public void updateTitle() {
		grid.setTitle("KISMET'S FOOD ADVENTURE!   yum total:  " + getScore() + " sunblockers left: "+ clickCount);
	}
	
	
   // return true if the game is finished, false otherwise
	//      used by play() to terminate the main game loop 
	public boolean isGameOver() {
		
		if(score <=-20)
		{
			System.out.println("Too much sun! must go back into my hide! \n game over, you lose");
			lost = true;//decides which splashscreen to put up
			return true;
		}
		else if(score >= 75)
		{
			System.out.println("wow what a great adventure! I sure am full! \n gameover, you win ");
			lost = false;//decides which splashscreen to put up
			return true;
		}
		
		return false;
	}
	
	public void splashScreen(String img){// this method was made to create the functionality of the splash screen
		grid.setSplash(img);
		boolean splash = true;
		
		while(splash)//very similar to pause
		{
			grid.sleep(200);
			Location click = grid.checkLastLocationClicked();
		
			if (click != null)
			{
				grid.setSplash(null);
				splash = false;
			}
			
		}
	}
	
}
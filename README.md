# javaproject
mport java.awt.Font;
import java.awt.Graphics;
import java.awt.Image;
import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;

import javax.swing.ImageIcon;
import javax.swing.JFrame;
import javax.swing.JPanel;

public class Project {
	JFrame frame = new JFrame("Strat Game");
	Image bg = new ImageIcon("배경.png").getImage();
	Image ch = new ImageIcon("캐릭터.png").getImage();
	Image fire= new ImageIcon("불.png").getImage();
	Image Tree= new ImageIcon("크리스마스.png").getImage();
	Image rock= new ImageIcon("돌.png").getImage();
	int WIDTH = 1200, HEIGHT=600;
	int FXS=70, FYS=70;
	int FX= 1000, FY=453,FIREWIDTH = FY+FYS;;
	int RX=1800;
	int x = 70, y = 60;
	int TX= 1400, TY=FY/2,TYD=FY/2+FYS;
	int playerX = 150, playerXF = playerX+30;
	int playerY = 453, playerFootY = playerY + 60;
	int xStep = -8;
	double yStep = 0;
	int jmpHeight =300;
	int jmpSpeed = 8;
	boolean jmp = false,test =true, end=false;
	int i=0;
	double a = 0.008, b=a+a;
	float timer=0.0f;
	int n=12;

	public Project() {
		frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		frame.setSize(WIDTH, HEIGHT);
		frame.getContentPane().add(new GamePanel());
		frame.addKeyListener(new GameListener());
		frame.setVisible(true);

	}

	public void go() {
		while (test) {
			timer += n / 1000.0f;
			i++;
			b+=a;	
			playerY += yStep;
			FX+=xStep-b; TX+=xStep-b; RX+=xStep-b;
			if (playerY <= 453 - jmpHeight) {
				yStep = jmpSpeed + 2 +b/2;
			}
			if (playerY >= 452) {
				yStep = 0;
				jmp = false;
			}
			if(FX <=0) {
				FX=1300;
			}
			if(TX<=0) {
				TX=1300;
			}
			if(RX<=0) {
				RX=1300;
			}
			if(((FX<=playerXF && FX>=playerX) || (TX<=playerXF && TX>=playerX)) && (FY<=playerY)) {
				xStep=0;
				end=true;
				test =false;
			}
			if((RX<=playerXF && RX>=playerX) && (TY<=playerY && TYD>=playerY)) {
				xStep=0;
				end=true;
				test=false;
			}
			try {
				Thread.sleep(n);
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			frame.repaint();
		}
	}


	class GamePanel extends JPanel {
		public void paintComponent(Graphics g) {
			g.drawImage(bg, 0, 0, 1200, 600, this);
			g.drawImage(ch, playerX, playerY, x, y, this);
			g.drawImage(fire, FX, FY, FXS, FYS, this);
			g.drawImage(Tree, TX, FY, FXS, FYS, this);
			g.drawImage(rock, RX, TY, FXS, FYS, this);
			g.setFont(new Font("바탕",Font.BOLD, 15));
			g.drawString("score:"+i, 10, 30);
			g.setFont(new Font("바탕",Font.BOLD, 15));
			g.drawString("time : "+(int) timer, 1100, 30);
			if(end==true) {
				g.setFont(new Font("진하게",Font.ITALIC, 50));
				g.drawString("Game Over",450,200);
				g.drawString("score:"+i, 475,300);
				g.drawString("time :"+(int) timer, 475,400);
			}
		}
	}

	class GameListener implements KeyListener {

		@Override
		public void keyPressed(KeyEvent e) {
			// TODO Auto-generated method stub
			switch (e.getKeyCode()) {
			case KeyEvent.VK_UP:
				if(!jmp) {
					yStep = -jmpSpeed-b ;
					jmp = true;
					break;
				}
			}
		}

		@Override
		public void keyTyped(KeyEvent e) {
			// TODO Auto-generated method stub	
		}

		@Override
		public void keyReleased(KeyEvent e) {
			// TODO Auto-generated method stub
		}

	}
}

# task-3
game" Rock,paper,..."
 КОД:
 
 
 import java.security.SecureRandom;
import java.util.InputMismatchException;
import java.util.Scanner;

public class Game {
    public static void main(String[] args) {

        SecureRandom secureRandom = new SecureRandom();
        int count = 0;
        if (args.length >= 3 && args.length % 2 == 1) {
            for (int i = 0; i < args.length; i++) {
                for (int j = i + 1; j < args.length; j++) {
                    if (args[i].equals(args[j])) {
                        count++;
                    }
                }
            }
            if (count != 0) {
                System.out.println("Moves must be unique");
                return;
            }
            System.out.println("Available moves:");
            for (int i = 0; i < args.length; i++) {
                System.out.println((i + 1) + " - " + args[i]);
                if (i == args.length - 1) {
                    System.out.println("0 - exit");
                }
            }
            int pc_move = 1 + secureRandom.nextInt(args.length - 1);
            System.out.print("Enter your move:");
            int sch=0;
            int player_move = func1();

            if (player_move!=0){
                while (sch != 1) {
                    if (player_move > 0 && player_move <= args.length) {
                        sch++;
                    } else {
                        System.out.println("Not available move");

                        player_move = func1();
                    }
                }
            }else {return;
            }
            System.out.println("Your move:" + args[player_move - 1]);
            System.out.println("Computer move:" + args[pc_move - 1]);
            if (pc_move > player_move && pc_move - player_move > args.length / 2) {
                System.out.println("You lose.");
            } else if (pc_move < player_move && player_move - pc_move <= args.length / 2) {
                System.out.println("You lose.");
            } else if (pc_move < player_move && player_move - pc_move > args.length / 2) {
                System.out.println("You win.");
            } else if (pc_move > player_move && pc_move - player_move <= args.length / 2) {
                System.out.println("You win.");
            } else {
                System.out.println("Draw.");
            }
        } else {
            System.out.println("The number of parameters must be odd and more than 3");
        }
    }

    public static int func1() {
        Scanner a = new Scanner(System.in);
        int c=0;
        try {
            c=a.nextInt();
        }catch (InputMismatchException inp){
            System.out.println("Move must be int");
            System.out.println("Your move:");
            return func1();
        }
        return c;
    }
}




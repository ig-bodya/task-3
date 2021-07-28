# task-3
game" Rock,paper,..."
 КОД:
 
 
import javax.crypto.KeyGenerator;
import javax.crypto.Mac;
import javax.crypto.SecretKey;
import javax.crypto.spec.SecretKeySpec;
import javax.xml.bind.DatatypeConverter;
import java.math.BigInteger;
import java.security.InvalidKeyException;
import java.security.NoSuchAlgorithmException;
import java.security.SecureRandom;
import java.util.InputMismatchException;
import java.util.Scanner;

public class Game {
    public static void main(String[] args) throws NoSuchAlgorithmException, InvalidKeyException {

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
            int pc_move = 1 + secureRandom.nextInt(args.length - 1);
            KeyGenerator keyGen = KeyGenerator.getInstance("AES");
            keyGen.init(secureRandom);
            SecretKey secretKey = keyGen.generateKey();
            String HMACkey = new BigInteger(1, secretKey.getEncoded()).toString(16);
            Mac sha256_HMAC = Mac.getInstance("HmacSHA256");
            sha256_HMAC.init(new SecretKeySpec(HMACkey.getBytes(), "HmacSHA256"));
            byte[] result = sha256_HMAC.doFinal(HMACkey.getBytes());
            System.out.println("HMAC: "+ DatatypeConverter.printHexBinary(result));
            System.out.println("Available moves:");
            for (int i = 0; i < args.length; i++) {
                System.out.println((i + 1) + " - " + args[i]);
                if (i == args.length - 1) {
                    System.out.println("0 - exit");
                }
            }


            System.out.print("Enter your move:");
            int sch=0;
            int player_move = func1();


                while (sch != 1) {
                    if (player_move != 0) {
                        if (player_move > 0 && player_move <= args.length) {
                            sch++;
                        } else {
                            System.out.println("Not available move");

                            player_move = func1();
                        }
                    }else {return;
                }
            }
            System.out.println("Your move:" + args[player_move - 1]);
            System.out.println("Computer move:" + args[pc_move - 1]);
            if (pc_move > player_move && pc_move - player_move > args.length / 2) {
                System.out.println("You lose.");
                System.out.println("HMAC KEY: "+HMACkey);
            } else if (pc_move < player_move && player_move - pc_move <= args.length / 2) {
                System.out.println("You lose.");
                System.out.println("HMAC KEY: "+HMACkey);
            } else if (pc_move < player_move && player_move - pc_move > args.length / 2) {
                System.out.println("You win.");
                System.out.println("HMAC KEY: "+HMACkey);
            } else if (pc_move > player_move && pc_move - player_move <= args.length / 2) {
                System.out.println("You win.");
                System.out.println("HMAC KEY: "+HMACkey);
            } else {
                System.out.println("Draw.");
                System.out.println("HMAC KEY: "+HMACkey);
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










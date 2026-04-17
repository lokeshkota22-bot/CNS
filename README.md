import javax.crypto.Cipher;
import javax.crypto.KeyGenerator;
import javax.crypto.SecretKey;
import javax.crypto.spec.IvParameterSpec;
import java.security.SecureRandom;
import java.util.Base64;
public class RijndaelAlgorithm {
 // Generate a 256-bit AES (Rijndael) secret key
 public static SecretKey generateKey() throws Exception {
 KeyGenerator keyGen = KeyGenerator.getInstance("AES");
 keyGen.init(256); // Rijndael key size: 128, 192, or 256 bits
 return keyGen.generateKey();
 }
 // Generate a random 16-byte Initialization Vector (IV)
 public static IvParameterSpec generateIV() {
 byte[] iv = new byte[16];
 new SecureRandom().nextBytes(iv);
 return new IvParameterSpec(iv);
 }
 // Encrypt plaintext using Rijndael (AES) with CBC mode
 public static String encrypt(String plainText, SecretKey key,
 IvParameterSpec iv) throws Exception {
 Cipher cipher = Cipher.getInstance("AES/CBC/PKCS5Padding");
 cipher.init(Cipher.ENCRYPT_MODE, key, iv);
 byte[] encryptedBytes = cipher.doFinal(plainText.getBytes("UTF-8"));
 return Base64.getEncoder().encodeToString(encryptedBytes);
 }
 // Decrypt ciphertext using Rijndael (AES) with CBC mode
 public static String decrypt(String cipherText, SecretKey key,
 IvParameterSpec iv) throws Exception {
 Cipher cipher = Cipher.getInstance("AES/CBC/PKCS5Padding");
 cipher.init(Cipher.DECRYPT_MODE, key, iv);
 byte[] decryptedBytes =
 cipher.doFinal(Base64.getDecoder().decode(cipherText));
 return new String(decryptedBytes, "UTF-8");
  }
 public static void main(String[] args) {
 try {
 String plainText = "This is AES Symmetric Algorithm";
 System.out.println("Original Text : " + plainText);
 // Generate Key and IV
 SecretKey secretKey = generateKey();
 IvParameterSpec iv = generateIV();
 System.out.println("Rijndael Key (Base64): " +
 Base64.getEncoder().encodeToString(secretKey.getEncoded()));
 System.out.println("IV (Base64) : " +
 Base64.getEncoder().encodeToString(iv.getIV()));
 // Encrypt
 String encrypted = encrypt(plainText, secretKey, iv);
 System.out.println("Encrypted Text : " + encrypted);
 // Decrypt
 String decrypted = decrypt(encrypted, secretKey, iv);
 System.out.println("Decrypted Text : " + decrypted);
 } catch (Exception e) {
 System.out.println("Error: " + e.getMessage());
 e.printStackTrace();
 }
 }
}

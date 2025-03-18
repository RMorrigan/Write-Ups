# PicoCTF25
## Cookie Monster’s Secret Recipe | picoCTF{c00k1e_m0nster_l0ves_c00kies_771D5EB0}
This one I understood the least. I clicked the link and used the inspection tool in the firefox browser to look at the cookies. Originally, there wasn't anything there. I decided to interact with the login screen and attempted username: ADMIN and password: password. I wasn't successfully logged in, but a base64 encoded flag appeared in the cookies. I used chat gpt to decode it.
## Rust fixme1 | picoCTF{4r3_y0u_4_ru$t4c30n_n0w?}
This is the first of 3 rust fixing challenges. There were small issues like missing semicolons.
```
use xor_cryptor::XORCryptor;

fn main() {
    // Key for decryption
    let key = String::from("CSUCKS"); // Statements in Rust end with semicolons
    
    // Encrypted flag values
    let hex_values = ["41", "30", "20", "63", "4a", "45", "54", "76", "01", "1c", "7e", "59", "63", "e1", "61", "25", "7f", "5a", "60", "50", "11", "38", "1f", "3a", "60", "e9", "62", "20", "0c", "e6", "50", "d3", "35"];
    
    // Convert the hexadecimal strings to bytes and collect them into a vector
    let encrypted_buffer: Vec<u8> = hex_values.iter()
        .map(|&hex| u8::from_str_radix(hex, 16).unwrap())
        .collect();
    
    // Create decryption object
    let res = XORCryptor::new(&key);
    if res.is_err() {
        return; // Corrected way to return in Rust
    }
    
    let xrc = res.unwrap();
    // Decrypt flag and print it out
    let decrypted_buffer = xrc.decrypt_vec(encrypted_buffer);
    
    // Corrected println with proper format specifier
    println!(
        "{}", // Use {} for default display formatting or {:?} for debug output
        String::from_utf8_lossy(&decrypted_buffer)
    );
}
```
## Rust fixme2 | picoCTF{4r3_y0u_h4v1n5_fun_y31?}
This is the second of 3 rust fixing challenges. There were issues like make the variables mutable.
```
use xor_cryptor::XORCryptor;

fn decrypt(encrypted_buffer:Vec<u8>, borrowed_string: &mut String){ // How do we pass values to a function that we want to change?

    // Key for decryption
    let key = String::from("CSUCKS");

    // Editing our borrowed value
    borrowed_string.push_str("PARTY FOUL! Here is your flag: ");

    // Create decrpytion object
    let res = XORCryptor::new(&key);
    if res.is_err() {
        return; // How do we return in rust?
    }
    let xrc = res.unwrap();

    // Decrypt flag and print it out
    let decrypted_buffer = xrc.decrypt_vec(encrypted_buffer);
    borrowed_string.push_str(&String::from_utf8_lossy(&decrypted_buffer));
    println!("{}", borrowed_string);
}

fn main() {
    // Encrypted flag values
    let hex_values = ["41", "30", "20", "63", "4a", "45", "54", "76", "01", "1c", "7e", "59", "63", "e1", "61", "25", "0d", "c4", "60", "f2", "12", "a0", "18", "03", "51", "03", "36", "05", "0e", "f9", "42", "5b"];

    // Convert the hexadecimal strings to bytes and collect them into a vector
    let encrypted_buffer: Vec<u8> = hex_values.iter()
        .map(|&hex| u8::from_str_radix(hex, 16).unwrap())
        .collect();

    let mut party_foul = String::from("Using memory unsafe languages is a: "); // Is this variable changeable?
    decrypt(encrypted_buffer, &mut party_foul); // Is this the correct way to pass a value to a function so that it can be changed?
}
```
## Rust fixme3 | picoCTF{n0w_y0uv3_f1x3d_1h3m_411}
This is the last of 3 rust fixing challenges. There were issues like using unsafe operations.
```
use xor_cryptor::XORCryptor;

fn decrypt(encrypted_buffer: Vec<u8>, borrowed_string: &mut String) {
    // Key for decryption
    let key = String::from("CSUCKS");

    // Editing our borrowed value
    borrowed_string.push_str("PARTY FOUL! Here is your flag: ");

    // Create decryption object
    let res = XORCryptor::new(&key);
    if res.is_err() {
        return;
    }
    let xrc = res.unwrap();

    // Did you know you have to do "unsafe operations in Rust?
    // https://doc.rust-lang.org/book/ch19-01-unsafe-rust.html
    // Even though we have these memory safe languages, sometimes we need to do things outside of the rules
    // This is where unsafe rust comes in, something that is important to know about in order to keep things in perspective
    
    unsafe {
        // Decrypt the flag operations 
        let decrypted_buffer = xrc.decrypt_vec(encrypted_buffer);

        // Creating a pointer 
        let decrypted_ptr = decrypted_buffer.as_ptr();
        let decrypted_len = decrypted_buffer.len();
        
        // Unsafe operation: calling an unsafe function that dereferences a raw pointer
        let decrypted_slice = std::slice::from_raw_parts(decrypted_ptr, decrypted_len);

        borrowed_string.push_str(&String::from_utf8_lossy(decrypted_slice));
    }
    println!("{}", borrowed_string);
}

fn main() {
    // Encrypted flag values
    let hex_values = ["41", "30", "20", "63", "4a", "45", "54", "76", "12", "90", "7e", "53", "63", "e1", "01", "35", "7e", "59", "60", "f6", "03", "86", "7f", "56", "41", "29", "30", "6f", "08", "c3", "61", "f9", "35"];

    // Convert the hexadecimal strings to bytes and collect them into a vector
    let encrypted_buffer: Vec<u8> = hex_values.iter()
        .map(|&hex| u8::from_str_radix(hex, 16).unwrap())
        .collect();

    let mut party_foul = String::from("Using memory unsafe languages is a: ");
    decrypt(encrypted_buffer, &mut party_foul);
}
```
## Hashcrack | picoCTF{UseStr0nG_h@shEs_&PaSswDs!_23622a7e}
After netcat into the challenge, I was presented three hashed passwords of increasing difficulty. MD5, SHA1 And SHA256, were each used and then I used the hashcat command to find the matching passwords.
## FANTASY CTF | picoCTF{m1113n1um_3d1710n_d5787049}
I used the netcat command to access the challenge. After being presented with a text adventure, I selected the options that led to the reveal of the flag.
![Picture1](https://github.com/user-attachments/assets/809133f9-0cb5-42e8-b133-adb3a9a155f2)

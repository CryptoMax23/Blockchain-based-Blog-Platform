module MyModule::BlogPlatform {
    use aptos_framework::signer;
    use aptos_framework::coin;
    use aptos_framework::aptos_coin::AptosCoin;

    /// Struct to represent a blog post.
    struct BlogPost has store, key {
        content: vector<u8>,  // Blog content stored as bytes
        author: address,      // Author's address
        reward: u64,          // Total reward received
    }

    /// Function to create a new blog post.
    public fun create_post(creator: &signer, content: vector<u8>) {
        let author_addr = signer::address_of(creator);
        let post = BlogPost {
            content,
            author: author_addr,
            reward: 0,
        };
        move_to(creator, post);
    }

    /// Function to reward the author of a blog post.
    public fun reward_author(sender: &signer, author: address, amount: u64) acquires BlogPost {
        let post = borrow_global_mut<BlogPost>(author);

        // Transfer tokens from sender to the author
        let reward_tokens = coin::withdraw<AptosCoin>(sender, amount);
        coin::deposit<AptosCoin>(author, reward_tokens);

        // Update the total reward
        post.reward = post.reward + amount;
    }
}


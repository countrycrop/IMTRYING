# IMTRYINGimport hashlib
import time

# Sample block data and parameters
block_data = "Sample Block Data"
max_nonce = 10000000  # Maximum nonce to try
target = "00000"  # Difficulty target: hash must start with 5 zeroes

def mine_sequential():
    """Sequential mining function to avoid threading issues."""
    nonce = 0
    start_time = time.time()  # Record the start time

    while nonce < max_nonce:
        try:
            # Concatenate block data with the current nonce
            data = block_data + str(nonce)
            # Calculate the SHA-256 hash of the data
            hash_result = hashlib.sha256(data.encode()).hexdigest()
            # Check if the hash meets the difficulty target
            if hash_result.startswith(target):
                end_time = time.time()  # Record the end time
                print(f"Mining successful! Nonce: {nonce}")
                print(f"Hash: {hash_result}")
                print(f"Time taken: {end_time - start_time:.2f} seconds")
                return True
            nonce += 1
        except Exception as e:
            print(f"Encountered an error: {e}")
    
    print("Mining failed to find a valid hash within the maximum nonce range.")
    return False

if __name__ == "__main__":
    mine_sequential()
    print("Mining completed.")

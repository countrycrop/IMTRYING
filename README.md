iMTRYING
import hashlib
import time
import threading

# Sample block data and parameters
block_data = "Sample Block Data"
max_nonce = 10000000  # Maximum nonce to try
target = "00000"  # Difficulty target: hash must start with 5 zeroes
num_threads = 4  # Number of threads to use for mining

# Flag to indicate if mining is successful
mining_successful = False
# Lock for printing to avoid jumbled output
print_lock = threading.Lock()

def mine(start_nonce, end_nonce):
    """Mining function to be run by each thread."""
    global mining_successful
    start_time = time.time()

    for nonce in range(start_nonce, end_nonce):
        if mining_successful:
            break  # Stop if mining is already successful
        try:
            data = block_data + str(nonce)
            hash_result = hashlib.sha256(data.encode()).hexdigest()
            if hash_result.startswith(target):
                end_time = time.time()
                with print_lock:
                    print(f"Mining successful! Nonce: {nonce}")
                    print(f"Hash: {hash_result}")
                    print(f"Time taken: {end_time - start_time:.2f} seconds")
                mining_successful = True
                return
        except Exception as e:
            with print_lock:
                print(f"Encountered an error: {e}")

def mine_parallel():
    """Main function to start multiple mining threads."""
    threads = []
    range_per_thread = max_nonce // num_threads

    for i in range(num_threads):
        start_nonce = i * range_per_thread
        end_nonce = (i + 1) * range_per_thread if i != num_threads - 1 else max_nonce
        thread = threading.Thread(target=mine, args=(start_nonce, end_nonce))
        threads.append(thread)
        thread.start()
    
    for thread in threads:
        thread.join()
    
    if not mining_successful:
        print("Mining failed to find a valid hash within the maximum nonce range.")

if __name__ == "__main__":
    mine_parallel()
    print("Mining completed.")

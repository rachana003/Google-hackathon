# Google-hackathon
import random

# Function to parse a line from the monitor output
def parse_line(line):
    parts = line.split()
    timestamp = int(parts[0])
    transaction_type = parts[1]
    data = parts[2]
    return timestamp, transaction_type, data

# Function to simulate throttling
def throttle():
    # Placeholder for throttling simulation
    pass

# Function to set maximum buffer size
def set_max_buffer_size(buffer_id, num_entries):
    # Placeholder for setting maximum buffer size
    print(f"Setting maximum buffer size for buffer {buffer_id} to {num_entries} entries.")

# Function to set arbiter weights
def set_arbiter_weights(agent_type, weight):
    # Placeholder for setting arbiter weights
    print(f"Setting arbiter weight for agent type {agent_type} to {weight}.")

# Function to adjust NOC design parameters
def adjust_noc_design():
    # Placeholder for adjusting NOC design parameters
    print("Adjusting NOC design parameters based on performance metrics.")

# Initialize variables
total_latency = 0
total_bytes_transferred = 0
total_transactions = 0
start_time = None

# Define constants
throttling_frequency = 0.05  # 5% of the time
throttling_cycles = 5  # Throttling happens for 5 cycles
min_latency = 10  # Example minimum latency
max_bandwidth = 1000  # Example maximum bandwidth

# Example interface monitor output
simulator_monitor_output = [
    "0 Rd 100000",
    "2 Wr Addr2",
    "4 Rd Addr3",
    "10 Wr Addr",
    # Add more lines as needed
]

# For each line in simulator monitor output
for line in simulator_monitor_output:
    # Parse timestamp, transaction type, and data
    timestamp, transaction_type, data = parse_line(line)

    # Check if transaction type is "Rd" or "Wr"
    if transaction_type == "Rd" or transaction_type == "Wr":
        # Check if start time is None
        if start_time is None:
            start_time = timestamp
        else:
            # Calculate latency
            latency = timestamp - start_time
            total_latency += latency
            start_time = None

        # Check if transaction type is "Rd"
        if transaction_type == "Rd":
            # Increment total bytes transferred by size of data
            total_bytes_transferred += len(data)

        # Increment total transactions
        total_transactions += 1

    # Apply throttling if needed
    if random.random() < throttling_frequency:
        # Throttle for throttling_cycles
        for _ in range(throttling_cycles):
            throttle()

# Calculate average latency and bandwidth
average_latency = total_latency / total_transactions
average_bandwidth = total_bytes_transferred / total_latency

# Check if latency and bandwidth meet requirements
if average_latency <= min_latency and average_bandwidth >= 0.95 * max_bandwidth:
    # Buffer sizing to support 90% occupancy
    all_buffers = [1, 2, 3]  # Assume three buffers
    for buffer_id in all_buffers:
        set_max_buffer_size(buffer_id, 900)  # Assume 900 entries for each buffer

    # Set arbiter weights
    arbiter_weights = {'CPU': 0.6, 'IO': 0.4}  # Assume weights for CPU and IO agents
    for agent_type, weight in arbiter_weights.items():
        set_arbiter_weights(agent_type, weight)
else:
    # Adjust NOC design parameters based on performance metrics
    adjust_noc_design()

# Debugging
print("Average Latency:", average_latency)
print("Average Bandwidth:", average_bandwidth)

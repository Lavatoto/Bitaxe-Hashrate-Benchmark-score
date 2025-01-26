- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - 

# Explanation of Changes Made to the Script
In this updated version of the script, several improvements were made to enhance the analysis of the Bitaxe's performance based on three key metrics: hashrate, temperature, and power consumption. Below is a summary of the main changes:

Score Calculation and Normalization: I added a feature to calculate a performance score for each voltage and frequency configuration. This score takes into account three main factors:

Average hashrate: The number of gigahashes per second (GH/s) produced by the machine.
Average temperature: The operating temperature of the system, which should stay within safe limits.
Energy efficiency: The amount of power consumed per tera-hash (J/TH), which is a crucial metric for evaluating power efficiency.
The score is calculated using the following formula:

```bash
score = (average_hashrate * 10) - (average_temperature * 2) - (efficiency_jth * 0.5)
```
This score prioritizes the hashrate, while penalizing high temperatures and low energy efficiency. The higher the score, the better the configuration performs.

Score Normalization: To make the scores comparable across different configurations, I added a normalization process. The score is now adjusted on a scale of 0 to 1000, allowing a relative evaluation of the best configurations:

```bash
normalized_score = score / (max_hashrate * 10 + max_temp * 2 + max_efficiency * 0.5) * 1000
```
This ensures a consistent scale where 1000 represents the best possible performance, based on the theoretical limits of the tested parameters (hashrate, temperature, and efficiency).

Ranking of Results: After the tests are complete, I implemented a ranking function based on the average hashrate, temperature, and efficiency. 
The top 5 configurations are extracted and displayed based on their normalized score. The ranking is done by sorting the results by average hashrate (from highest to lowest), making it easier to identify the best-performing configurations.

Displaying Results: The results are displayed in a detailed table, including:

The voltage and frequency of each tested configuration.
The average hashrate, average temperature, and energy efficiency.
The normalized score out of 1000, providing a quick visual reference for the relative performance of each configuration.
Additionally, the results are saved in a JSON file, allowing for historical tracking of the tests performed.

Conclusion
These modifications add an extra layer to the analysis of Bitaxe performance. Not only do they provide raw results for each configuration, but they also offer a ranking system based on key criteria like power consumption and thermal stability. With these improvements, it's easier to identify the optimal configuration for a balance between performance and energy efficiency.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - 


# Bitaxe Hashrate Benchmark

A Python-based benchmarking tool for optimizing Bitaxe mining performance by testing different voltage and frequency combinations while monitoring hashrate, temperature, and power efficiency.

## Features

- Automated benchmarking of different voltage/frequency combinations
- Temperature monitoring and safety cutoffs
- Power efficiency calculations (J/TH)
- Automatic saving of benchmark results
- Graceful shutdown with best settings retention
- Docker support for easy deployment

## Prerequisites

- Python 3.11 or higher
- Access to a Bitaxe miner on your network
- Docker (optional, for containerized deployment)

## Installation

### Standard Installation

1. Clone the repository:
```bash
git clone https://github.com/mrv777/Bitaxe-Hashrate-Benchmark.git
cd Bitaxe-Hashrate-Benchmark
```

2. Create and activate a virtual environment:
```bash
python -m venv venv
# On Windows
venv\Scripts\activate
# On Linux/Mac
source venv/bin/activate
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```

### Docker Installation

1. Build the Docker image:
```bash
docker build -t bitaxe-benchmark .
```

## Usage

### Standard Usage

Run the benchmark tool by providing your Bitaxe's IP address:

```bash
python bitaxe_hashrate_benchmark.py <bitaxe_ip>
```

Optional parameters:
- `-v, --voltage`: Initial voltage in mV (default: 1150)
- `-f, --frequency`: Initial frequency in MHz (default: 500)

Example:
```bash
python bitaxe_hashrate_benchmark.py 192.168.2.26 -v 1200 -f 550
```

### Docker Usage

Run the container with your Bitaxe's IP address:

```bash
docker run --rm bitaxe-benchmark <bitaxe_ip> [options]
```

Example:
```bash
docker run --rm bitaxe-benchmark 192.168.2.26 -v 1200 -f 550
```

## Configuration

The script includes several configurable parameters:

- Maximum chip temperature: 66°C
- Maximum VR temperature: 90°C
- Maximum allowed voltage: 1400mV
- Maximum allowed frequency: 1200MHz
- Benchmark duration: 20 minutes
- Sample interval: 30 seconds
- Voltage increment: 25mV
- Frequency increment: 25MHz

## Output

The benchmark results are saved to `bitaxe_benchmark_results.json`, containing:
- Complete test results for all combinations
- Top 5 performing configurations ranked by hashrate
- For each configuration:
  - Average hashrate
  - Temperature readings
  - Power efficiency metrics (J/TH)
  - Voltage/frequency combinations tested

## Safety Features

- Automatic temperature monitoring with safety cutoff (66°C chip temp)
- Voltage regulator (VR) temperature monitoring with safety cutoff (90°C)
- Graceful shutdown on interruption (Ctrl+C)
- Automatic reset to best performing settings after benchmarking
- Input validation for safe voltage and frequency ranges
- Hashrate validation to ensure stability

## Benchmarking Process

The tool follows this process:
1. Starts with user-specified or default voltage/frequency
2. Tests each combination for 20 minutes
3. Validates hashrate is within 8% of theoretical maximum
4. Incrementally adjusts settings:
   - Increases frequency if stable
   - Increases voltage if unstable
   - Stops at thermal or stability limits
5. Records and ranks all successful configurations
6. Automatically applies the best performing stable settings

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the GNU General Public License v3.0 - see the [LICENSE](LICENSE) file for details.

## Disclaimer

Please use this tool responsibly. Overclocking and voltage modifications can potentially damage your hardware if not done carefully. Always ensure proper cooling and monitor your device during benchmarking.

Log Parser
This project processes log data and analyzes each entry using a lookup table. It generates a CSV output file with two main sections:

Tag Counts: Provides counts for each tag.
Port/Protocol Combination Counts: Lists counts for each port/protocol combination.
Files
logs.txt: The input file containing flow log data. (Supports only version 2 and skips invalid or other version entries).
lookup_table.csv: A CSV file with columns dstport, protocol, and tag.
protocol_map.csv: A file mapping protocol numbers to protocol names.
output_results.csv: The generated output file that contains tag counts and port/protocol combination counts.
Usage
Compilation and Execution
To run the project, navigate to the /src directory containing the Java files.

Compile the Java Files:

Compile all .java files in the directory using:
bash
Copy code
javac *.java
Run the Program:

To run with default filenames:

bash
Copy code
java LogParser
To run with custom filenames by specifying command-line arguments:

bash
Copy code
java LogParser --lookup custom_lookup.txt --logs custom_logs.txt --protocol custom_protocol_map.csv --output custom_results.csv
Command-Line Arguments
The program supports four optional command-line arguments, providing flexibility for custom file inputs:

--lookup: Specifies the path to the lookup table file (default: lookup_table.txt).
--logs: Specifies the path to the flow log file (default: logs.txt).
--protocol: Specifies the path to the protocol map CSV file (default: protocol_map.csv).
--output: Specifies the path to the output CSV file (default: output_results.csv).
Assumptions
Given proficiency in Java, the project is implemented in Java without using third-party libraries. Key assumptions are:

Handling Multiple Tags for Port/Protocol Combinations
If a port/protocol combination maps to multiple tags, all matching tags are stored and counted.

Protocol Map File
Instead of hardcoding protocol number-to-name mappings, the project uses an up-to-date protocol map file downloaded from the IANA website:

Download Protocol Numbers from IANA
The CSV file includes two columns: Protocol Number and Name, allowing the system to stay updated with any new protocols.

Considering Both Source and Destination Ports
To address ambiguity in the output requirements, the program considers both source and destination ports when calculating port/protocol counts.

CustomPair Class
A CustomPair class was created to efficiently handle pairs of values, specifically (port, protocol) pairs. This class is essential for correctly mapping and counting unique port/protocol combinations.

Time Complexity Analysis
loadLookupTable: O(N), where N is the number of rows in the lookup table.
loadProtocolMap: O(M), where M is the number of rows in the protocol map file.
parseFlowLogs: O(L), where L is the number of lines in the log file. Each line is processed in constant time for map updates, making the overall complexity linear relative to the number of log entries.
saveResults: O(T + P), where T is the number of unique tags and P is the number of unique port/protocol combinations.
Named Arguments for Flexibility
The program supports named arguments to enhance user-friendliness, making it suitable for both quick tests and more detailed analyses with custom inputs.

Testing Scenarios
Valid Log Data: Ensured the program outputs correct tag counts and port/protocol counts.
No Matching Lookup Data: Handled cases where there were no matching dstport/protocol combinations, correctly displaying untagged counts.
Case Sensitivity: Verified correct counts when the protocol field's case differed in the lookup table.
Empty Log File: The output file contained no tag or port/protocol counts.
Corrupt Log Entries: Skipped invalid log entries and processed only valid version 2 entries, producing accurate results.
Empty Lookup Table: Displayed only untagged counts and corresponding port/protocol entries for valid logs.

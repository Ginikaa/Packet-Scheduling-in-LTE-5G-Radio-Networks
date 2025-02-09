# LTE/5G Network Packet Scheduling Analysis

## Project Overview
This assignment focused on the analysis of three packet scheduling methods in LTE/5G networks, focusing on evaluating multi-user and frequency diversity gains, and examining efficiency-fairness trade-offs in resource allocation.

## Network Configuration
- 37 omnidirectional cells in hexagonal layout
- Inter-site distance: 1.0 km
- Each cell:
  - 50 Scheduling Resource Blocks (SRBs), each 180 kHz-wide
  - Continuous transmission at full power of 20 Watt, equally split over 50 SRBs
  - Frequency reuse factor: N = 1 (contiguous reuse)
- Path gain model: -(115 dB + 35 × log₁₀(distance/km))
- User placement: 10 users (numbered 0-9) positioned at equal distances from cell center to edge
- Multipath fading: Modeled using 5000-TTI traces (5 seconds, 1ms per TTI)

## Implementation Structure

### 1. Single User Analysis (User 0)
- SINR calculation across all SRBs
- Conversion of SINR to attainable bit rates using Shannon formula
- Aggregate bit rate calculation over 50 SRBs
- Time-averaged experienced bit rate over 5000 TTIs

### 2. Multi-User Analysis (Users 0-9)
- Extension of single-user analysis to all users
- Comparative analysis of aggregate bit rates
- Time-averaged throughput analysis for each user

### 3. Packet Scheduling Implementation
Starting with users 8-9, progressively adding users (7-8-9, 6-7-8-9, etc.) until all users included.

**Time Domain Scheduling:**
1. Round Robin (RR)
   - Assigns all SRBs to single user per TTI
   - Fixed sequence (8-9-8-9-8-9...)

2. Maximum Rate (MR)
   - Assigns all SRBs to user with highest aggregate bit rate per TTI

3. Proportional Fair (PF)
   - Assigns all SRBs to user with best ratio of current to average throughput
   - Uses time-averaged bit rates from initial analysis

**Time/Frequency Domain Scheduling:**
1. Round Robin (RR)
   - Sequential SRB assignment across users
   - Continuous across TTI boundaries

2. Maximum Rate (MR)
   - Assigns each SRB to user with highest achievable bit rate
   - Multiple users can receive SRBs within same TTI

3. Proportional Fair (PF)
   - SRB assignment based on per-user throughput ratios
   - Considers individual SRB performance

## Performance Metrics
- Individual user throughput
- Aggregate cell throughput
- Jain's fairness index measuring:
  - Throughput fairness
  - Resource fairness

## Analysis Results

### Multi-user Diversity Gains
- Analyzed impact of adding users from cell edge to center
- Maximum Rate showed highest gain (1428% from 2 to 10 users)
- Proportional Fair achieved 343% gain
- Round Robin demonstrated 493% gain

### Frequency Diversity Benefits
Average cell throughput improvements:
- Maximum Rate: 14.06%
- Proportional Fair: 25.48%
- Round Robin: -0.03%

### Efficiency-Fairness Trade-offs
Detailed analysis of:
- Throughput fairness vs cell throughput
- Resource fairness vs cell throughput
- Impact of user count on fairness metrics

## Technologies Used
- Python for implementation and analysis
- Statistical analysis tools
- Mathematical modeling

## Skills Demonstrated
- Wireless network simulation
- Resource allocation algorithms
- Performance optimization
- Data analysis and visualization
- Technical documentation

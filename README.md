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
- Multipath fading: Modeled using 5000-TTI traces (5 seconds, 1ms per TTI) across each SRB for each user.

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
   - Assigns all SRBs to user with highest aggregate attainable bit rate per TTI

3. Proportional Fair (PF)
   - Assigns all SRBs to user with best ratio of current instantaneous bitrate to time-averaged bitrate

**Time/Frequency Domain Scheduling:**
1. Round Robin (RR)
   - Sequential SRB assignment across users
   - Continuous across TTI boundaries

2. Maximum Rate (MR)
   - Assigns each SRB to user with highest achievable bit rate
   - Multiple users can receive multiple SRBs within same TTI

3. Proportional Fair (PF)
   - SRB assignment based on per-user throughput ratios. 
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
- Maximum Rate showed highest gain in cell throughput (1428% from 2 to 10 users) but this had low fairness because users closer to the base station(which had better channel conditions) were assigned majority of the time.
- Proportional Fair achieved 343% gain - Though there was some gain in adding users closer to the base station, it was not as high as MR. This is because proportional fairness does not assign to the highest instantaneous bitrate, but considers the ratio with respect to their average. Hence at some times it would assign the resource to a poor user even though their bitrate with respect to their average is the best at that time. 
- Round Robin demonstrated 493% gain - This also has similar gains with the PF. There is some gain in adding users closer to the base station, but this doesnt take into consideration users with good channel conditions.

### Frequency Diversity Benefits
Average cell throughput improvements compared to only time domain:
- Maximum Rate: 14.06% - In the Maximum Rate scheduling algorithm, resources are allocated to users with the highest instantaneous bit rate. This would benefit from frequency diversity by selecting SRBs where the channel conditions are optimal.
- Proportional Fair: 25.48% - Likewise for the PF, frequency diversity increases the likelihood of assigning resources to SRBs with less fading, thus ensuring that users with instantaneous good bit rates compared to their average receive more resources, thereby improving the throughput while maintaining fairness over time.
- Round Robin: -0.03% - nO gains. As a result of assigning resources to users sequentially, the algorithm cannot take
advantage of the peaks in channel quality at the different SRBs.

### Efficiency-Fairness Trade-offs
Detailed analysis of:
- Throughput fairness vs cell throughput (Results showed an inverse proportionality)
- Resource fairness vs cell throughput (Results showed an inverse proportionality)




# AWS Cross-Region VPC Peering

**EWB Courses | AWS Cloud Internship | Internship Project**

This project demonstrates cross-region VPC peering between two Amazon Virtual Private Clouds (VPCs) in Mumbai (`ap-south-1`) and N. Virginia (`us-east-1`). It covers VPC networking, subnet design, route tables, security groups, EC2 instances, and private connectivity.

## Project Information

| Detail | Information |
|---|---|
| Student Name | Affan Ahmed Shaikh |
| Batch | 1 |
| Project Title | AWS VPC Peering |
| Training Provider | EWB Courses |
| Course | AWS Architect Associate |
| Regions | Mumbai and N. Virginia |
| Completion Date | 18 October 2026 |

## Architecture Overview

The architecture consists of two VPCs connected through an active cross-region VPC peering connection.

- **VPC A:** Mumbai (`ap-south-1`)
- **VPC B:** N. Virginia (`us-east-1`)
- **Peering Connection:** `pcx-00e94484adb9f5689`
- **Connectivity:** Private communication between resources using private IP addresses, route tables, and security group rules.

### Architecture Diagram

<img width="1536" height="1024" alt="AWS Multi-Region VPC Peering Architecture" src="https://github.com/user-attachments/assets/26ffe0b2-1704-4ef0-8682-7e35c9bcc9fa" />


## AWS Resources

### 1. VPC A — Mumbai

| Resource | Name / Value |
|---|---|
| VPC Name | `ewb-vpc-a` |
| VPC ID | `vpc-057388798f595834b` |
| CIDR Block | `10.10.0.0/16` |
| Region | `ap-south-1` |
| Internet Gateway | `ewb-vpc-a-IGW` |
| NAT Gateway | `ewb-a-NAT` |
| Security Group | `ewb-sg-vpc-a` |
| Security Group ID | `sg-05d75876f25ace280` |

**Subnets**

| Subnet | Availability Zone | CIDR |
|---|---|---|
| `ewb-a-public` | `ap-south-1a` | `10.10.1.0/24` |
| `ewb-a-private` | `ap-south-1b` | `10.10.2.0/24` |

**EC2 Instances**

| Instance | Instance ID | Type | IP Address |
|---|---|---|---|
| `ewb-bastion` | `i-0b88931ffd25a0581` | `t3.micro` | Public: `65.0.102.243` |
| `ewb-app-a(private)` | `i-092c80a6e28d393eb` | `t3.micro` | Private: `10.10.2.131` |

### 2. VPC B — N. Virginia

| Resource | Name / Value |
|---|---|
| VPC Name | `ewb-vpc-b` |
| VPC ID | `vpc-0ddde312bbdb98f32` |
| CIDR Block | `10.20.0.0/16` |
| Region | `us-east-1` |
| Internet Gateway | `ewb-vpc-b-IGW` |
| NAT Gateway | `ewb-vpc-b-NAT` |
| Security Group | `ewb-sg-vpc-b` |
| Security Group ID | `sg-066f128902377cb3f` |

**Subnets**

| Subnet | Availability Zone | CIDR |
|---|---|---|
| `ewb-b-public` | `us-east-1a` | `10.20.1.0/24` |
| `ewb-b-private` | `us-east-1b` | `10.20.2.0/24` |

**EC2 Instance**

| Instance | Instance ID | Type | Private IP |
|---|---|---|---|
| `ewb-app-b(private)` | `i-0e1c81b93de8b8dc6` | `t3.micro` | `10.20.2.131` |

The private application instance has no public IP address.

## VPC Peering Connection

| Property | Details |
|---|---|
| Peering Connection ID | `pcx-00e94484adb9f5689` |
| Status | Active |
| Requester VPC | `ewb-vpc-a` |
| Requester CIDR | `10.10.0.0/16` |
| Requester Region | `ap-south-1` |
| Accepter VPC | `ewb-vpc-b` |
| Accepter CIDR | `10.20.0.0/16` |
| Accepter Region | `us-east-1` |

## Route Tables

The project contains three route tables in each VPC, including an unassociated route table.

| VPC | Route Table | Subnet Association |
|---|---|---|
| Mumbai | `ewb-a-publicRT` | `ewb-a-public` |
| Mumbai | `ewb-a-privateRT` | `ewb-a-private` |
| Mumbai | `rtb-01dd9e65bb1cdd19e` | None |
| N. Virginia | `ewb-b-publicRT` | `ewb-b-public` |
| N. Virginia | `ewb-b-privateRT` | `ewb-b-private` |
| N. Virginia | `rtb-0d8b624bdab7253d3` | None |

The route tables must contain the appropriate local routes, internet or NAT routes, and remote VPC routes where required. The actual route destinations and targets should be verified in the AWS console.

## Security Groups

### Mumbai — `ewb-sg-vpc-a`

Security Group ID: `sg-05d75876f25ace280`

The documented inbound rules include:

- All traffic from `0.0.0.0/0`.
- All traffic from the same security group.

### N. Virginia — `ewb-sg-vpc-b`

Security Group ID: `sg-066f128902377cb3f`

The documented inbound rules include:

- SSH over TCP port `22` from `5.110.4.183/32`.
- All traffic from `10.10.0.0/16`.
- All traffic from the same security group.

**Security note:** These are the rules recorded in the project report. Restrict access to only the required protocols, ports, and trusted sources before using this architecture in a production environment.

## Project Objectives

- Create VPCs in two different AWS regions.
- Configure public and private subnets.
- Deploy EC2 instances in the appropriate subnets.
- Configure Internet Gateways and NAT Gateways.
- Establish a cross-region VPC peering connection.
- Configure route tables and subnet associations.
- Configure security groups for controlled network access.
- Test private connectivity and troubleshoot network issues.
- Document the architecture and resource inventory.

## Testing and Validation

The project report includes terminal screenshots documenting testing activities.

The validation checklist covers:

1. Verify the peering connection is active.
2. Inspect route tables and subnet associations.
3. Test connectivity between private IP addresses.
4. Test SSH access to the remote private application instance through the bastion host.
5. Test ICMP failure and recovery after changing the relevant rules.
6. Record the results and document cleanup activities.

Record actual command outputs and results before describing any test as successful.

## Key Learnings

- Understanding VPC CIDR blocks and subnet design.
- Configuring cross-region VPC peering.
- Understanding route propagation requirements and return routes.
- Applying security group rules to control network traffic.
- Using a bastion host to access private instances.
- Troubleshooting routing, SSH, and ICMP connectivity.
- Documenting cloud architecture and validation evidence.

## Repository Structure

```text
aws-vpc-peering/
├── README.md
├── aws-vpc-peering-architecture.png
└── screenshots/
    ├── vpc-resources.png
    ├── peering-connection.png
    ├── security-groups.png
    └── connectivity-tests.png
```

Add only the files that you actually upload to the repository.

## Cleanup

After completing the project:

- Terminate EC2 instances that are no longer required.
- Delete NAT Gateways after checking their dependencies.
- Release unused Elastic IP addresses.
- Remove temporary security group rules.
- Delete the VPC peering connection and associated resources only when they are no longer needed.
- Verify that billable resources have been removed.

## Author

**Affan Ahmed Shaikh**  
AWS Cloud Internship — EWB Courses  
Batch 1

**Project:** AWS Cross-Region VPC Peering  
**Completion Date:** 18 October 2026

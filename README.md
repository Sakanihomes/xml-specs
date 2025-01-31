# Property Feed XML Specifications

This document outlines the XML feed specifications for property listings. The feed must be a valid XML file with UTF-8 encoding.

## Root Element

The root element `<list>` contains all property listings and has two mandatory attributes:

| Attribute | Description | Format | Required |
|-----------|-------------|---------|----------|
| last_update | Date and time of last activity | YYYY-MM-DD HH:MM:SS | ✅ |
| listing_count | Total number of properties | Integer | ✅ |

Example:
```xml
<list last_update="2024-01-31 13:14:15" listing_count="834">
```

## Property Element

Each property is represented by a `<property>` element with the following fields:

### Required Fields

| Field | Description | Format/Values |
|-------|-------------|---------------|
| last_update | Last activity timestamp | YYYY-MM-DD HH:MM:SS |
| reference_number | Unique identifier | Alphanumeric, no spaces |
| offering_type | Type of offering | RR = Residential Rent<br>RS = Residential Sale<br>CR = Commercial Rent<br>CS = Commercial Sale |
| property_type | Property category | AP = Apartment/Flat<br>VH = Villa/House<br>etc. (see full list below) |
| size | Floor area size | Number (no separators) |
| title | Marketing heading | Text |
| description | Property details | CDATA wrapped text |
| city | Emirate | Text (e.g., Dubai, Abu Dhabi) |
| community | District/project area | Text |
| photo | Property images | At least 1 photo required (see Photos section) |
| agent | Agent information | Nested element (see Agent section) |

### Optional Fields

| Field | Description | Format/Values |
|-------|-------------|---------------|
| price | Property price | For Sales: Number<br>For Rentals: Nested element |
| service_charge | Yearly service charge | Number |
| cheques | Number of rental cheques | 1-12 |
| sub_community | Project/sub-area | Text |
| property_name | Building name | Text |
| furnished | Furnishing status | Yes/No/Partly |
| view360 | 360° tour URL | Valid HTTPS URL |
| video_tour_url | YouTube video URL | Valid YouTube URL |

### Property Types (Full List)
- AP - Apartment/Flat
- BU - Bulk Units
- BW - Bungalow
- CD - Compound
- DX - Duplex
- FA - Factory
- FM - Farm
- FF - Full Floor
- HA - Hotel Apartment
- HF - Half Floor
- LC - Labor Camp
- LP - Land/Plot
- OF - Office Space
- BC - Business Centre
- PH - Penthouse
- RE - Retail
- RT - Restaurant
- ST - Storage
- TH - Townhouse
- VH - Villa/House
- SA - Staff Accommodation
- WB - Whole Building
- SH - Shop
- SR - Showroom
- CW - Co-working Space
- WH - Warehouse

### Amenities

#### Private Amenities
Comma-separated list of available amenities:
```
AC - Central A/C & Heating
BA - Balcony
BK - Built-in Kitchen Appliances
BL - View of Landmark
BW - Built-in Wardrobes
CP - Covered Parking
CS - Concierge Service
LB - Lobby in Building
MR - Maid's Room
MS - Maid Service
PA - Pets Allowed
PG - Private Garden
PJ - Private Jacuzzi
PP - Private Pool
PY - Private Gym
VC - Vastu-compliant
SE - Security
SP - Shared Pool
SS - Shared Spa
ST - Study
SY - Shared Gym
VW - View of Water
WC - Walk-in Closet
CO - Children's Pool
PR - Children's Play Area
BR - Barbecue Area
```

#### Commercial Amenities
Comma-separated list of available amenities:
```
CR - Conference Room
AN - Available Networked
DN - Dining in building
LB - Lobby in Building
SP - Shared Pool
SY - Shared Gym
CP - Covered Parking
VC - Vastu-compliant
PN - Pantry
MZ - Mezzanine
```

### Agent Section

Required nested element with agent details:

```xml
<agent>
    <id>AGENT_ID</id>          <!-- Required: Unique identifier -->
    <name>AGENT_NAME</name>    <!-- Required: Full name -->
    <email>EMAIL</email>       <!-- Required: Valid email -->
    <phone>PHONE</phone>       <!-- Required: Digits only -->
    <photo>URL</photo>         <!-- Optional: Valid image URL -->
    <license_no>NUMBER</license_no> <!-- Optional: RERA number -->
    <info>TEXT</info>          <!-- Optional: Additional info -->
</agent>
```

Specifications:
- Accepted formats: jpg, jpeg, png, bmp, gif
- Recommended dimensions: 800x600 to 1920x1080 pixels
- Maximum file size: 1MB per image
- Maximum 10 images per listing

### Price Format

For Sales:
```xml
<price>2500000</price>
```

For Rentals:
```xml
<price>
    <yearly>100000</yearly>
    <monthly>12000</monthly>
    <weekly>3000</weekly>
    <daily>500</daily>
</price>
```

Minimum Values:
- Residential Sale: 100,000
- Commercial Sale: 100,000
- Residential Rent (Yearly): 10,000
- Residential Rent (Monthly): 1,000
- Residential Rent (Weekly): 1,000
- Residential Rent (Daily): 100
- Commercial Rent (Yearly): 10,000
- Commercial Rent (Monthly): 1,000

## Additional Fields

### Project Information
| Field | Description | Required | Format |
|-------|-------------|----------|---------|
| project_name | Real estate project name | No | Text |
| completion_status | Project status | No | completed/off_plan/completed_primary/off_plan_primary |
| developer | Developer company name | No | Text |

### Property Details
| Field | Description | Required | Format |
|-------|-------------|----------|---------|
| title_deed | Title deed number | No | n-digit-number/year (e.g., 123456/2024) |
| availability_date | Move-in date for rentals | No | YYYY-MM-DDTHH:MM:SSZ |
| geopoints | Property coordinates | No | longitude,latitude |

## Example XML

```xml
<list last_update="2024-01-31 13:14:15" listing_count="1">
    <property last_update="2024-01-31 13:14:15">
        <reference_number>PROP123</reference_number>
        <offering_type>RS</offering_type>
        <property_type>AP</property_type>
        <price>2500000</price>
        <size>1500</size>
        <title_en>Luxury 2BR Apartment in Downtown</title_en>
        <description_en><![CDATA[Beautiful apartment with amazing views...]]></description_en>
        <city>Dubai</city>
        <community>Downtown Dubai</community>
        <photo>
            <url last_updated="2024-01-31 14:15:16">https://example.com/image1.jpg</url>
            <url last_updated="2024-01-31 14:15:16">https://example.com/image2.jpg</url>
        </photo>
        <agent>
            <id>AG123</id>
            <name>John Doe</name>
            <email>john@example.com</email>
            <phone>501234567</phone>
        </agent>
    </property>
</list>
```

## Notes

1. All date/time stamps must follow the format: YYYY-MM-DD HH:MM:SS
2. Numbers should not contain any separators (commas, spaces)
3. Text content should not contain unconventional special characters
4. CDATA sections must be used for description fields
5. URLs must be valid and use HTTPS
6. Images must follow size and format requirements
7. Required fields must not be empty

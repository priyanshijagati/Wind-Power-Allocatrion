# Wind-Power-Allocatrion
• Utilized the ALV Grid Control to render wind power allocation data, incorporating interactive functionalities such as sorting and filtering, thereby enhancing data visualization and user experience for end-users.

• Developed dynamic selection screens, facilitating the extraction, aggregation, and manipulation of wind power data.


# Power Generation & Consumption Report (SAP ABAP)

This SAP ABAP report is designed to fetch, calculate, and display information related to power generation and consumption from a custom database. It integrates multiple tables and function modules to retrieve data, perform calculations, and display it in a user-friendly ALV grid format.

## Project Overview

The purpose of this report is to streamline the process of retrieving power generation, consumption, allocation, and surplus data for various customers. It uses SAP's native database tables and custom function modules to fetch and process data, making it easier for businesses to manage energy allocation and usage information effectively.

## Features

- **Custom Selection Screen**: Allows users to input specific plant, customer, and billing information for targeted data retrieval.
- **Data Fetching**: The report retrieves data from the following tables:
  - `ZPOWER_GEN`: Stores power generation and consumption data.
  - `KNVV`, `KNVP`, `KNA1`: Stores customer details (Ship-to, Sold-to, etc.).
  - `T001L`: Stores plant information.
  - `ADRNC`: Stores billing information.
- **Function Modules**: Calls several custom function modules to retrieve:
  - Generated power (`ZSDWNID_GETGEN`).
  - Consumed power (`ZSDWNID_GETCONS`).
  - Wheeling factor (`ZSDWIND_GETALL`).
- **Surplus Calculation**: Calculates surplus energy based on generated, allocated, and consumed data.
- **ALV Grid Display**: Outputs results using SAP's ALV (ABAP List Viewer) for better presentation and user interaction.

## Requirements

- **SAP ABAP Development System**
- **SAP ERP System** with access to the following tables:
  - `ZPOWER_GEN`, `KNVV`, `KNVP`, `KNA1`, `T001L`, `ADRNC`
- Custom function modules: `ZSDWNID_GETGEN`, `ZSDWNID_GETCONS`, `ZSDWIND_GETALL`

## Installation and Setup

1. **Download the Report**: Clone the repository or copy the code from the source.
   
2. **Upload to SAP**: Use transaction `SE38` to create a new report and paste the code from the report file.
   
3. **Create Custom Tables**: Ensure that the custom tables such as `ZPOWER_GEN` and `ZSDWIND_ALLOC` exist in the system. Populate these tables with relevant data.
   
4. **Activate the Function Modules**: The function modules `ZSDWNID_GETGEN`, `ZSDWNID_GETCONS`, and `ZSDWIND_GETALL` should be created and activated in the system.

5. **Run the Report**: After activation, execute the report by providing necessary parameters in the selection screen.

## Code Structure

- **Selection Screen**: The user is prompted to enter the following:
  - Plant Code
  - Month and Year of the data
  - Sold-to and Bill-to customer details
- **Data Fetching**: The report fetches address numbers and customer information based on the provided billing and shipping details.
- **Function Module Integration**: Custom function modules are called to retrieve power generation, consumption, and wheeling data.
- **Calculation Logic**: Surplus energy is calculated by subtracting the consumed energy from the allocated energy, where allocated energy is derived based on a wheeling factor.
- **ALV Grid Display**: The results are presented in an interactive ALV grid, allowing users to view the details of the data fetched.

### Example Code Snippet

```abap
FORM fetch_data.
  " Retrieve address number from ADRC table
  SELECT SINGLE addrnumber
    INTO addrnumber
    FROM adrc
    WHERE roomnumber = bill_to.

  IF sy-subrc <> 0.
    MESSAGE 'Bill-to address not found' TYPE 'E'.
    RETURN.
  ENDIF.

  " Retrieve ship-to customer number from KNA1
  SELECT kunnr
    INTO TABLE it_ship_to
    FROM kna1
    WHERE adrnr = addrnumber
      AND ktokd = '0002'.


# Ford Motor GraphQL Schema

## Overview

This conceptual GraphQL schema covers the Ford Motor Company connected vehicle and developer API surface, based on the Ford Developer Portal (https://developer.ford.com/). It models FordConnect capabilities — remote commands, vehicle status queries, EV charging, navigation, fleet management, service history, dealer locator, and authentication — as a unified graph.

## Schema Source

- Provider: Ford Motor Company
- Developer Portal: https://developer.ford.com/
- APIs: FordConnect (https://developer.ford.com/apis/fordconnect), Ford WLTP Emissions (https://developer.ford.com/emissions)
- Schema Type: Conceptual (derived from public documentation)

## Root Types

### Query

Fetch vehicle profiles, status, location, trips, charging data, alerts, maintenance records, diagnostics, dealers, and inventory.

### Mutation

Execute remote commands (lock, unlock, remote start/stop, charge control), update settings, register webhooks.

### Subscription

Stream real-time vehicle alerts, location updates, charging progress, and status changes via webhook-backed subscriptions.

## Type Summary

| Category | Types |
|---|---|
| Vehicle Identity | Vehicle, ConnectedVehicle, VehicleProfile, VIN, Make, Model, Trim, Year, Color |
| Vehicle Status | VehicleStatus, IgnitionStatus, DoorStatus, WindowStatus, LockStatus |
| EV / Charging | ChargeStatus, BatteryLevel, PlugStatus, ChargeSettings, ChargingSchedule, ChargingLocation, ChargingStation, ChargingPoint, ChargingNetwork, EVSE, EVRange, EVRangeEstimate |
| Fuel | FuelLevel, FuelEfficiency |
| Remote Commands | RemoteStart, RemoteStop |
| FordPass | FordPass, FordPassPower, FordPassConnect |
| Location / Navigation | Location, GPSCoordinate, NavigationRoute, PointOfInterest |
| Trips | Trip, TripHistory, TripDetail, DriveHistory |
| Alerts | Alert, VehicleAlert |
| Maintenance | MaintenanceReminder, OilChange, ServiceHistory, ServiceRecord, TireStatus |
| Diagnostics | DiagnosticCode, DTCCode, Recall |
| Odometer | OdoReading |
| Dealer / Inventory | Dealer, DealerLocator, DealerInventory, NewVehicle, UsedVehicle |
| Auth | APIKey, Token, Webhook |

## Usage Notes

- Authentication uses OAuth 2.0 tokens issued by the Ford Developer Portal. The `Token` type models access and refresh tokens.
- `ConnectedVehicle` extends `Vehicle` with live telemetry fields available only for FordPass-enrolled vehicles.
- Remote commands (`RemoteStart`, `RemoteStop`) return a `CommandResult` with a `requestId` that can be polled or awaited via subscription.
- EV types (`ChargeStatus`, `EVRange`, `ChargingSchedule`) apply to Ford electric and plug-in hybrid models.
- `DTCCode` and `DiagnosticCode` expose OBD-II diagnostic trouble codes surfaced through FordConnect.
- `Recall` data is sourced from NHTSA integration exposed through the developer portal.
- `DealerLocator` supports geo-radius queries using `GPSCoordinate`.

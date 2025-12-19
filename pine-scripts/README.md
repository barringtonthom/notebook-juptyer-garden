# Support & Resistance Zone Indicator

A comprehensive TradingView Pine Script v6 indicator that creates dynamic support and resistance zones based on pivot points.

## Features

### Core Functionality

1. **Dynamic Zone Creation**
   - Automatically detects pivot highs and lows
   - Creates support zones at pivot lows
   - Creates resistance zones at pivot highs
   - Zones extend infinitely into the future (configurable)

2. **Zone Detection**
   - Detects when price enters zones
   - Visual markers for zone entry
   - Touch counter for each zone

3. **Rejection Alerts**
   - Identifies when price rejects from zones
   - Potential reversal signals
   - Configurable rejection detection sensitivity

4. **Smart Zone Management**
   - Three behavior modes when zones are broken:
     - **Hide**: Remove broken zones
     - **Mark as Broken**: Gray out broken zones
     - **Flip Role**: Convert resistance to support (and vice versa)

## Input Parameters

### Pivot Settings
- **Pivot Lookback Length** (default: 10)
  - Number of bars to look back for pivot detection
  - Range: 1-100

- **Pivot Strength** (default: 5)
  - Number of bars on each side for pivot validation
  - Higher values = fewer but stronger pivots
  - Range: 1-50

### Zone Settings
- **Zone Width (%)** (default: 0.5%)
  - Percentage width of each zone around pivot price
  - Range: 0.1-5.0%

- **Maximum Zones to Display** (default: 5)
  - Maximum number of support and resistance zones shown
  - Range: 1-20

- **Extend Zones to Future** (default: true)
  - Whether zones extend infinitely to the right

### Zone Behavior
- **Behavior When Broken** (default: "Hide")
  - **Hide**: Removes broken zones from chart
  - **Mark as Broken**: Shows broken zones in gray
  - **Flip Role**: Converts broken resistance to support (and vice versa)

- **Flip Threshold (%)** (default: 2.0%)
  - How far price must move beyond zone to flip it
  - Only applies when "Flip Role" is selected
  - Range: 0.5-10.0%

### Visual Settings
- **Resistance Zone Color** (default: red with transparency)
- **Support Zone Color** (default: green with transparency)
- **Broken Zone Color** (default: gray with transparency)
- **Zone Border Width** (default: 1)
- **Show Zone Labels** (default: true)
  - Display R1, R2, S1, S2 labels on zones

### Alert Settings
- **Alert on Zone Entry** (default: true)
  - Trigger alert when price enters a zone

- **Alert on Zone Rejection** (default: true)
  - Trigger alert when price rejects from a zone

- **Rejection Detection Bars** (default: 3)
  - Number of bars to confirm rejection
  - Range: 1-10

## Visual Elements

### Zone Boxes
- **Resistance Zones**: Red boxes above price
- **Support Zones**: Green boxes below price
- **Broken Zones**: Gray boxes (if "Mark as Broken" is selected)

### Price Action Markers
- **▼** Small red triangle: Price entered resistance zone
- **▲** Small green triangle: Price entered support zone
- **✕** Orange cross above: Price rejected from resistance (bearish reversal signal)
- **✕** Blue cross below: Price rejected from support (bullish reversal signal)

### Information Table
- Located in top-right corner
- Shows:
  - Number of active resistance zones
  - Number of active support zones
  - Total touches for each zone type

## How It Works

### 1. Zone Creation
The indicator identifies significant pivot points using the `ta.pivothigh()` and `ta.pivotlow()` functions. When a pivot is confirmed:
- A zone is created with width defined by the Zone Width parameter
- The zone extends from the pivot bar into the future
- Zone is added to the active zones array

### 2. Zone Entry Detection
On each bar, the indicator checks if the current close price is within any active zone:
- If price enters a zone for the first time, it increments the touch counter
- A visual marker appears on the chart
- An alert is triggered (if enabled)

### 3. Rejection Detection
The indicator looks for rejection patterns:
- **Resistance Rejection**: Price touches zone from below, then moves back down
- **Support Rejection**: Price touches zone from above, then moves back up
- Confirmation requires price to move away from zone within the specified number of bars

### 4. Zone Breaking & Management
When price breaks through a zone:
- **Hide Mode**: Zone is deleted from chart
- **Mark as Broken Mode**: Zone changes to gray color but remains visible
- **Flip Role Mode**: If price moves far enough (Flip Threshold), the zone converts to opposite type

## Usage Guide

### Basic Setup
1. Copy the code from `support_resistance_zones.pine`
2. Open TradingView and navigate to Pine Editor
3. Paste the code and click "Add to Chart"

### Recommended Settings by Trading Style

#### Day Trading (Intraday)
- Pivot Lookback Length: 5-10
- Pivot Strength: 3-5
- Zone Width: 0.3-0.5%
- Rejection Detection Bars: 2-3

#### Swing Trading (Multi-day)
- Pivot Lookback Length: 10-20
- Pivot Strength: 5-10
- Zone Width: 0.5-1.0%
- Rejection Detection Bars: 3-5

#### Position Trading (Long-term)
- Pivot Lookback Length: 20-50
- Pivot Strength: 10-20
- Zone Width: 1.0-2.0%
- Rejection Detection Bars: 5-10

### Setting Up Alerts
1. Right-click on chart → "Add Alert"
2. Condition: Select the indicator
3. The script has built-in alert messages:
   - "Price entered Resistance/Support Zone"
   - "Price REJECTED from zone - Potential reversal"

## Trading Strategies

### Strategy 1: Zone Rejection Trading
1. Wait for price to enter a zone
2. Look for rejection signal (✕ marker)
3. Enter trade in direction of rejection
4. Place stop loss beyond the zone
5. Take profit at next zone level

### Strategy 2: Zone Breakout Trading
1. Identify strong zones with multiple touches
2. Wait for clean break of zone
3. If using "Flip Role" mode, watch for retest of flipped zone
4. Enter on successful retest
5. Ride trend to next zone

### Strategy 3: Zone Confluence Trading
1. Look for areas where multiple zones overlap
2. These represent stronger support/resistance levels
3. Take trades at these high-probability zones
4. Use smaller position sizes at isolated zones

## Technical Details

### Pine Script Version
- Version: 6 (Latest)
- Uses User Defined Types (UDT) for zone storage
- Maximum boxes: 500
- Maximum lines: 500

### Data Structures
The indicator uses a custom Zone type with properties:
- Box ID for visual representation
- Pivot price and zone boundaries
- Zone type (resistance/support)
- Break status and activity flag
- Touch counter and last touch bar

### Performance Considerations
- Limits number of active zones to prevent performance issues
- Removes old zones when maximum is reached
- Uses efficient array operations
- Optimized for real-time bar updates

## Customization Ideas

### Color Schemes
Modify the default colors to match your chart theme:
```pine
resistanceColor = input.color(color.new(#FF0000, 85), "Resistance Zone Color")
supportColor = input.color(color.new(#00FF00, 85), "Support Zone Color")
```

### Zone Strength Indicator
Add zone strength based on number of touches:
- Zones with more touches could have different colors
- Stronger zones could have thicker borders

### Multi-Timeframe Analysis
Combine zones from multiple timeframes:
- Daily zones for major support/resistance
- 4H zones for swing levels
- 1H zones for intraday levels

## Troubleshooting

### No Zones Appearing
- Check if Pivot Strength is too high
- Reduce Pivot Lookback Length
- Ensure chart has enough historical data

### Too Many Zones
- Increase Pivot Strength for stronger pivots only
- Reduce Maximum Zones to Display
- Increase Zone Width to merge similar levels

### Zones Breaking Too Quickly
- Increase Zone Width
- Increase Flip Threshold
- Use "Mark as Broken" instead of "Hide"

## License

This indicator is provided under the Mozilla Public License 2.0.

## Version History

### v1.0 (2025-12-19)
- Initial release
- Dynamic zone creation from pivot points
- Zone entry and rejection detection
- Three zone break behaviors
- Comprehensive alert system
- Information table with statistics

## Contributing

To improve this indicator:
1. Fork the repository
2. Make your changes
3. Test thoroughly on different markets and timeframes
4. Submit a pull request with description of improvements

## Disclaimer

This indicator is for educational and informational purposes only. It does not constitute financial advice. Always do your own research and risk management before trading.

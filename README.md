# Discharge Curve Digitizer

A small browser tool that reads a battery cell's voltage discharge chart and turns it into a table of voltage vs. state of charge (SoC). The table can be pasted straight into Excel, so you can look up the voltage at any SoC instead of estimating it by eye from the datasheet.

It was built for checking battery cells for the EcoCar high voltage battery pack.

## What it does

You give it a picture of a discharge chart, like the ones in cell datasheets. You show it where the axes are and click on the curves you care about. It follows each curve automatically and gives you the voltage at every SoC step you choose (1% by default).

It works with charts that plot voltage against SoC, depth of discharge, or capacity discharged in Ah or mAh. For Ah charts it converts to SoC using the cell's rated capacity, or using the point where each curve ends.

It also writes an Excel formula that blends between table rows. That way any SoC you type gives a voltage, even values that fall between rows.

## How to use it

There is nothing to install. Download `Discharge_Curve_Digitizer.html` and open it in Edge, Chrome or Firefox. It runs entirely in the browser and does not upload anything.

1. **Load a chart.** Take a screenshot of the chart (Win+Shift+S) and press Ctrl+V on the page. You can also drag an image file onto the page or use the file picker. If the chart is in a PDF, zoom in on it before taking the screenshot, since more pixels means more accurate readings.
2. **Calibrate the axes.** Pick the X-axis type. Click "Set X1" and then click a labeled gridline or tick on the X-axis. Do the same for X2, Y1 and Y2, then type the value of each point. Points that are far apart give the best accuracy. The magnifier in the corner helps you click the exact pixel.
3. **Trace the curves.** Click "Auto-trace" and then click on a curve. It follows that color across the chart, including dashed and dotted lines. Click more curves to add them and give each one a name, like "0.2C". If a trace wanders onto the legend or another curve, try lowering the color tolerance or drawing a box with "Limit trace area". You can also delete bad points with "Erase box" and add your own with "Add/remove points".
4. **Check the result.** White dots on the chart mark the exact values that go into the table. If they sit on the curves, the numbers are good.
5. **Export.** Set the SoC step and order, then click "Copy for Excel" and paste into your sheet. You can also download a CSV file.

"Save project" saves the image, calibration and curves to a file so you can come back later and change the step size or add curves.

## Looking up any SoC in Excel

The table only has rows at each step, such as 37% and 38%. The formula in step 5 of the tool fills in the gaps. To set it up you tell the tool two cells.

- **Table starts at** is the top-left cell where you pasted the table, which is the "SoC" header cell. If you pasted the table at A1, leave it as A1.
- **SoC input cell** is the cell that will hold the SoC you want to look up, such as H2.

Copy the formula and paste it into any other cell. Now when you type 37.4 into H2, that cell shows the voltage at 37.4% SoC. It works the same if H2 holds a value calculated elsewhere in your sheet, so you can point it at your pack's minimum SoC and the voltage updates on its own.

SoC values beyond the ends of the table return the voltage at the nearest end rather than an error. The formula uses only FORECAST, OFFSET and MATCH, so it works in any version of Excel and in Google Sheets.

## Accuracy

Accuracy depends mostly on the size of the image. The tool shows how much one pixel is worth once the axes are calibrated. On typical datasheet charts that is about 0.002 to 0.004 V per pixel. Each traced point is placed at the center of the line, which gets slightly better than one pixel.

As a check, it read the 0.2C curve of one test cell as 2.697 V at 10% SoC, 2.892 V at 20% and 3.391 V at 50%. The values pulled directly from the datasheet PDF's vector data were 2.70 V, 2.89 V and 3.39 V.

A few things to keep in mind.

- Gray cells in the table are SoC values outside the part of the curve that was traced, for example below the point where a high C-rate curve hits its cutoff voltage. These hold the voltage at the end of the curve.
- Where curves of similar colors overlap, the tracer can jump between them. Look at the white dots and fix any spots that are off.
- The chart shows voltage under load at the plotted C-rate. It is not the resting open circuit voltage. The lowest C-rate curve is the closest stand-in for open circuit voltage.

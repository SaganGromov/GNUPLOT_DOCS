
# Comprehensive Guide to Gnuplot (.gp scripts)

Gnuplot is a powerful command-line-driven graphing utility. This guide provides detailed instructions on how to create `.gp` scripts to generate various plots, fit data, and export your work.

---

## 1. Getting Started: Running .gp Scripts

A Gnuplot script is a simple text file with a `.gp` extension containing a sequence of Gnuplot commands.

### Running a Script
You can run a script from your terminal in two main ways:

1.  **Directly from the command line:**
    ```bash
    gnuplot your_script_name.gp
    ```
    This command will execute the script. If the script is configured to output to a file, the plot will be saved. To see the plot in a window and keep it open, add the `-p` (persist) flag:
    ```bash
    gnuplot -p your_script_name.gp
    ```

2.  **From within the Gnuplot interactive session:**
    Start Gnuplot by typing `gnuplot` in your terminal. Then, use the `load` command:
    ```gnuplot
    gnuplot> load 'your_script_name.gp'
    ```

### Basic Script Structure
A simple script to plot the sine function looks like this:

**`simple_plot.gp`**
```gnuplot
# Set the title of the plot
set title "Simple Sine Function"

# Set the labels for the x and y axes
set xlabel "x"
set ylabel "sin(x)"

# Plot the function sin(x)
plot sin(x)

# Use 'pause -1' to keep the plot window open until it's closed manually
# This is useful for interactive viewing.
pause -1 "Press Enter or close the window to exit"
````

-----

## 2\. Plotting: Functions and Data Files

The core command in Gnuplot is `plot` for 2D graphs and `splot` for 3D graphs.

### Plotting Functions

You can plot any of Gnuplot's built-in mathematical functions.

  * **Single function:**
    ```gnuplot
    plot sin(x)
    ```
  * **Multiple functions:** Separate them with a comma.
    ```gnuplot
    plot sin(x) title 'Sine', cos(x) title 'Cosine'
    ```

### Customizing the Plot

Gnuplot offers extensive customization options.

  * **Titles and Labels:**
    ```gnuplot
    set title "My Custom Plot" font "Verdana,14"
    set xlabel "X-Axis Label"
    set ylabel "Y-Axis Label"
    ```
  * **Axis Range (`xrange`, `yrange`):**
    ```gnuplot
    set xrange [-10:10]  # Sets x-axis range from -10 to 10
    set yrange [-1.5:1.5] # Sets y-axis range from -1.5 to 1.5
    ```
  * **Logarithmic Scale:**
    ```gnuplot
    set logscale x  # Logarithmic x-axis
    set logscale y  # Logarithmic y-axis
    unset logscale  # Removes all log scales
    ```
  * **Grid:**
    ```gnuplot
    set grid
    ```
  * **Legend (`key`):**
    ```gnuplot
    set key top left       # Position the legend
    set key box            # Draw a box around the legend
    unset key              # Disable the legend
    ```

### Plotting Data from Files (.dat)

To plot data from a file, you use the `plot` command followed by the filename in quotes. Assume you have a data file named `data.dat`:

**`data.dat`**

```
# X      Y
1        2.3
2        4.1
3        5.9
4        8.2
5        10.3
```

  * **Basic Plot:**

    ```gnuplot
    # Gnuplot assumes column 1 is x and column 2 is y by default
    plot 'data.dat' title 'My Data'
    ```

  * **Specifying Columns (`using`):**
    If your data has more columns, use the `using` directive. For a file like this:
    **`data_with_errors.dat`**

    ```
    # X   Y    Y_error
    1   2.3    0.2
    2   4.1    0.3
    3   5.9    0.4
    4   8.2    0.5
    5   10.3   0.6
    ```

    You can specify which columns to use for x, y, and errors.

    ```gnuplot
    # Plot column 1 vs column 2
    plot 'data_with_errors.dat' using 1:2 title 'Data'

    # Plot column 1 vs column 3
    plot 'data_with_errors.dat' using 1:3 title 'Errors'
    ```

-----

## 3\. Plot Styles and Customization

Gnuplot can generate many different styles of plots using the `with` keyword.

  * `lines` (default): Connects points with lines.
  * `points`: Shows only points.
  * `linespoints`: Shows both lines and points.
  * `impulses`: Draws a vertical line from the x-axis to each point.
  * `errorbars` or `yerrorbars`: For plotting data with error values.
  * `pm3d`: For creating 2D color maps (heatmaps).

### Style Examples

  * **Lines and Points:**

    ```gnuplot
    # Customize line type (lt), line width (lw), point type (pt), and point size (ps)
    plot 'data.dat' with linespoints lt 1 lw 2 pt 7 ps 1.5
    ```

      * `lt`: line type (integer, depends on terminal)
      * `lw`: line width (number)
      * `pt`: point type (integer, different numbers are different shapes)
      * `ps`: point size (number)

  * **Error Bars:**
    Requires a data file with at least three columns (x, y, y\_error).

    ```gnuplot
    # Plot data with y-error bars
    plot 'data_with_errors.dat' using 1:2:3 with yerrorbars title 'Data with Errors'
    ```

### 3D Plotting (`splot`)

For 3D data or functions, use `splot`.

**`3d_data.dat`**

```
1 1 5
1 2 6
1 3 7

2 1 6
2 2 7
2 3 8

3 1 7
3 2 8
3 3 9
```

*Note: Blank lines are important for Gnuplot to understand the grid structure.*

```gnuplot
# Plot a 3D function
splot x**2 + y**2

# Plot 3D data from a file as a surface
splot '3d_data.dat' with lines

# Create a 2D heatmap from 3D data
set view map # View from top-down
splot '3d_data.dat' with pm3d title 'Heatmap'
```

-----

## 4\. Data Fitting

Gnuplot can perform non-linear least-squares fitting of a user-defined function to a data set. The key command is `fit`.

### Fitting Process

1.  **Define the function:** Create a function with parameters you want to fit.

    ```gnuplot
    f(x) = m*x + c  # A linear function
    ```

2.  **Provide initial guesses:** Give the parameters reasonable starting values. This helps the algorithm converge.

    ```gnuplot
    m = 1.0
    c = 0.5
    ```

3.  **Run the `fit` command:**

    ```gnuplot
    # Syntax: fit f(x) 'datafile' using (columns) via (parameters)
    fit f(x) 'data.dat' using 1:2 via m, c
    ```

    Gnuplot will print the fitting results to the console, including the final parameter values, their standard errors, and the correlation matrix.

4.  **Plot the results:** Plot the original data and the fitted function together.

    ```gnuplot
    set title "Linear Fit of Data"
    set xlabel "X Value"
    set ylabel "Y Value"
    plot 'data.dat' using 1:2 title 'Experimental Data', f(x) title 'Fitted Line'
    ```

### Complete Fitting Script Example

**`fit_example.gp`**

```gnuplot
# 1. Define the function to fit
f(x) = m*x + c

# 2. Provide initial parameter guesses
m = 1
c = 1

# 3. Perform the fit
# The output of the fit process will be shown in the terminal
fit f(x) 'data.dat' using 1:2 via m, c

# 4. Plot the data and the fitted function
set title "Data Fitting Example"
set xlabel "X"
set ylabel "Y"
set key top left

# Create a more descriptive title for the fitted function using its final parameters
fit_title = sprintf("f(x) = %.2fx + %.2f", m, c)

plot 'data.dat' with points pt 7 ps 1 title 'Data Points', \
     f(x) with lines lw 2 title fit_title

pause -1
```

-----

## 5\. Generating Output Files (PNG, PDF, etc.)

To save your plot to a file instead of displaying it in a window, you need to set the **terminal** and the **output** file.

### The `set terminal` and `set output` Workflow

1.  **Set the terminal:** This tells Gnuplot what kind of file to create (e.g., `pngcairo`, `pdfcairo`, `svg`). The `cairo`-based terminals generally produce higher-quality graphics.
2.  **Set the output file:** This specifies the name of the file to save the plot to.
3.  **Issue your plot command(s).** All subsequent `plot` or `splot` commands will be directed to this file.
4.  **Unset the output (optional but good practice):** `unset output` closes the file.

### Example: Generating a PNG file

```gnuplot
# === SCRIPT TO GENERATE A PNG FILE ===

# 1. Set the terminal type and options
# 'size' is in pixels. 'font' sets the default font.
set terminal pngcairo size 800,600 enhanced font "Verdana,10"

# 2. Set the output file name
set output 'my_beautiful_plot.png'

# --- Plot Customization ---
set title "My Saved Plot"
set xlabel "Time (s)"
set ylabel "Amplitude"
set grid

# 3. Issue the plot command
plot 'data.dat' using 1:2 with linespoints title 'Measurement 1', \
     sin(x) with lines lw 2 title 'Theory'

# 4. Unset the output to close the file
# If you don't do this, the file may be corrupted or empty.
unset output

# You can now set another terminal/output if you want
# For example, to go back to the default interactive window:
# set terminal wxt
```

### Common Terminal Types

  * **PNG:** `set terminal pngcairo size <width,height>`
  * **PDF:** `set terminal pdfcairo size <width_in,height_in>` (size is in inches)
  * **SVG:** `set terminal svg size <width,height>`
  * **EPS (for LaTeX):** `set terminal postscript eps enhanced color`

<!-- end list -->

```
```

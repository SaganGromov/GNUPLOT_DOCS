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

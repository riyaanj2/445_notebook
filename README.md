\documentclass[11pt]{article}
\usepackage{amsmath}
\usepackage{graphicx}
\usepackage{hyperref}
\usepackage[margin=1in]{geometry}
\usepackage{longtable}
\usepackage{fancyhdr}
\pagestyle{fancy}
\fancyhf{}
\rhead{ECE 445 - Team 20}
\lhead{Automatic Vinyl Record Flipper}
\rfoot{Page \thepage}

\title{445 Lab Notebook}
\author{Riyaan Jain (riyaanj2)}
\date{May 2025}

\begin{document}

\maketitle

\section*{Lab Notebook – Automatic Vinyl Record Flipper}

\textbf{Team 20 – ECE 445 Senior Design, Spring 2025} \\
\textbf{Members:} Riyaan Jain, Mohammed Alkawai, Alfredo Velasquez \\
\textbf{TA:} Chi Zhang

\section{Overview}

This project automates the flipping of 7-inch vinyl records after playback, eliminating the need for manual intervention. It uses:
\begin{itemize}
    \item An \textbf{ESP32} microcontroller
    \item \textbf{Three servo motors} (2×20kg, 1×35kg torque)
    \item A \textbf{Hall effect sensor} and \textbf{N52 magnet} on the tonearm
    \item A custom 2-layer PCB designed in \textbf{KiCad}
\end{itemize}

The system lifts the tonearm, flips the record, and reseats it within \textbf{17 seconds}, maintaining a flipping accuracy of \textbf{100\%} across 50+ consecutive cycles.

\section{Weekly Logs \& Summaries}

\subsection*{Weeks 1–2: Ideation \& Design Exploration}
\begin{itemize}
    \item Identified problem: interruption due to manual record flipping
    \item Proposed solutions: rotating tray, robotic arm, flipping claw (chosen)
    \item Chose ESP32 for multitasking and PWM capabilities
    \item Investigated tonearm detection methods (ultrasonic vs. Hall sensor)
\end{itemize}

\subsection*{Weeks 3–4: Schematic \& PCB Design}
\begin{itemize}
    \item Designed power rail using LP2950-CZ1 and verified:
    \[
    C \geq \frac{I \cdot \Delta t}{\Delta V} = \frac{0.1 \cdot 1 \times 10^{-6}}{0.1} = 1\mu F
    \]
    \item Added bulk decoupling (10\,$\mu$F) and bypass capacitors (0.1\,$\mu$F)
    \item Used IPC-2221 standard to size high-current servo traces (80 mils)
    \item Designed servo headers, ESP32 pinout, Hall sensor filtering
\end{itemize}

\subsection*{Weeks 5–6: Assembly \& Mechanical Tuning}
\begin{itemize}
    \item Soldered and validated all components on PCB
    \item 3D printed clamping arms and adapter cone
    \item Verified servo torque:
    \[
    \tau = m \cdot g \cdot r = 0.18 \cdot 9.81 \cdot 0.15 \approx 0.265 \text{ Nm}
    \]
    DSServo provides 0.92 Nm torque $\Rightarrow$ 3.5$\times$ safety margin
\end{itemize}

\subsection*{Weeks 7–8: Firmware and Signal Integration}
\begin{itemize}
    \item Programmed ESP32 to generate PWM (GPIO 15, 16, 18)
    \item Used a state machine: \texttt{IDLE $\rightarrow$ LIFT $\rightarrow$ ROTATE $\rightarrow$ RESEAT $\rightarrow$ RESET}
    \item Added debounce for Hall effect detection on GPIO5
    \item Verified PWM pulse widths via oscilloscope
\end{itemize}

\subsection*{Weeks 9–10: System Testing}
\begin{itemize}
    \item Ran 50+ flips continuously – 100\% success rate
    \item Voltage stability over 20s:
    \[
    \text{Mean} = 4.94\,V,\quad \text{Std Dev} = \pm 0.02\,V
    \]
    \item Hall sensor triggered consistently at $\sim$0.62 inches (150 Gauss threshold)
    \item Partner Aaron supported mechanical tuning
\end{itemize}

\subsection*{Week 11: Optimization}
\begin{itemize}
    \item Added soft delays between servo stages to reduce current spikes
    \item Noted minor scratches from clamp – plan to add rubber lining
\end{itemize}

\section{Engineering Justifications}

\subsection*{Power Stability}
\begin{itemize}
    \item Target voltage tolerance: $\pm$0.1 V
    \item Achieved: $\pm$0.05 V fluctuation during max load
\end{itemize}

\subsection*{PWM Signal Design}
\begin{itemize}
    \item Frequency: 50 Hz
    \item Pulse width range: 500 µs (0°) to 2500 µs (180°)
\end{itemize}

\subsection*{Sensor Range}
\begin{itemize}
    \item Magnet emits $\sim$150 G at 0.62"
    \item Sensor threshold: 50 G (activated reliably within 1 inch)
\end{itemize}

\section{Figures and Diagrams}

\begin{itemize}
    \item \textbf{Figure 1:} Block Diagram – system overview
    \item \textbf{Figure 2:} Completed prototype on Crosley turntable
    \item \textbf{Figure 3:} Power schematic (KiCad)
    \item \textbf{Figure 4:} Magnetic field vs. distance curve
    \item \textbf{Figure 5:} Final PCB layout
\end{itemize}

(Insert images here using \verb!\includegraphics[width=\textwidth]{filename}!)

\section{Tolerance Analysis}

\begin{itemize}
    \item Max servo draw: 2.1 A, lab supply: 2.5 A (safe)
    \item Servo margin of error (angular): $\pm$1.5°
    \item Tonearm detection error: 0/50 trials
    \item Reseating misalignment: <1 mm (with tapered cone assist)
\end{itemize}

\section{Ethics and Safety}

\begin{itemize}
    \item Complied with IEEE 1100 and university lab safety rules
    \item Wore PPE during soldering and testing
    \item Avoided falsifying data or inflating success rates
    \item Handled magnets safely to avoid interference or snapping
\end{itemize}

\section{TA Feedback and Improvements}

\begin{itemize}
    \item Week 4: Switched from ultrasonic to Hall sensor (suggested)
    \item Week 6: Addressed jitter in servo response
    \item Week 10: Identified need for clamp padding
\end{itemize}

\section{Conclusion}

This system meets all core design requirements for an automatic record flipper:
\begin{itemize}
    \item Seamless, accurate flipping in <17s
    \item 100\% tonearm detection reliability
    \item No overheating or power instability
\end{itemize}
Planned improvements include a soft clamp and compatibility with larger records.

\section{Appendix: Verification Table}

\begin{longtable}{|p{0.4\textwidth}|p{0.4\textwidth}|p{0.1\textwidth}|}
\hline
\textbf{Requirement} & \textbf{Test Method} & \textbf{Result} \\
\hline
5V stable voltage & Multimeter over 20s & Pass \\
Servo torque > 0.3 Nm & Calculated via torque formula & Pass \\
Clamp alignment & Manual + video inspection & Pass \\
Hall effect detection & Magnet test at 0.62" & Pass \\
PWM accuracy & Oscilloscope pulse width & Pass \\
System stability & 10 min full-load test & Pass \\
Reseating accuracy & Visual + <1mm misalignment & Pass \\
False triggering & Tested in idle position & Pass \\
\hline
\end{longtable}

\end{document}

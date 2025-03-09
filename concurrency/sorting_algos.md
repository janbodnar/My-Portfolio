# Concurrency with sorting algorithms


## Threading with progress bars

Sorting is CPU-bound, so the overall so the sum of runtimes is not efficient.  
Also to show progress with progressbars, the tasks are artificially delayed, which distorts 
the true measurements. 

On the other hand comparing sorting algorithms with threading gives some overall picture of  
relative differences between different algorithms.  

```python
import tkinter as tk
from tkinter import ttk
import time
import random
import threading

class SortingApp(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Sorting Algorithms")
        self.geometry("800x600")
        
        # Set dark theme
        self.configure(bg='#222222')
        style = ttk.Style()
        style.theme_use('clam')
        style.configure('TFrame', background='#222222')
        style.configure('TLabel', background='#222222', foreground='#ffffff', font=('Helvetica', 14))
        style.configure('TButton', background='#404040', foreground='#ffffff', font=('Helvetica', 14))
        style.map('TButton', background=[('active', '#606060')])
        style.configure('TProgressbar', background='#00cc00', troughcolor='#404040')
        
        # Initialize variables
        self.array = [random.randint(1, 100) for _ in range(500)]
        self.sorting = False
        self.stop_flag = threading.Event()
        
        # UI Elements - Progress Bars with Percentage
        self.progress_bars = {}
        self.percent_labels = {}
        algorithms = ["Quick Sort", "Insertion Sort", "Radix Sort", "Bubble Sort", "Selection Sort"]
        
        for algo in algorithms:
            frame = ttk.Frame(self)
            frame.pack(pady=10, padx=20, fill='x')
            
            label = ttk.Label(frame, text=f"{algo}:")
            label.pack(side='left', padx=5)
            
            progress = ttk.Progressbar(frame, length=500, maximum=100)
            progress.pack(side='left', fill='x', expand=True)
            self.progress_bars[algo] = progress
            
            percent_label = ttk.Label(frame, text="0%", width=10)
            percent_label.pack(side='left', padx=5)
            self.percent_labels[algo] = percent_label
        
        # Button Frame
        button_frame = ttk.Frame(self)
        button_frame.pack(pady=20)
        
        # Buttons in one row
        self.start_button = ttk.Button(button_frame, text="Start", command=self.start_sorting)
        self.start_button.pack(side='left', padx=5)
        
        self.stop_button = ttk.Button(button_frame, text="Stop", command=self.stop_sorting)
        self.stop_button.pack(side='left', padx=5)
        
        self.resume_button = ttk.Button(button_frame, text="Resume", command=self.resume_sorting)
        self.resume_button.pack(side='left', padx=5)
        
        self.threads = {}

    # Sorting algorithms
    def quick_sort(self, arr):
        def partition(low, high):
            if self.stop_flag.is_set():
                return -1
            i = low - 1
            pivot = arr[high]
            for j in range(low, high):
                if arr[j] < pivot:
                    i += 1
                    arr[i], arr[j] = arr[j], arr[i]
            arr[i + 1], arr[high] = arr[high], arr[i + 1]
            return i + 1

        def quicksort(low, high):
            if self.stop_flag.is_set():
                return
            if low < high:
                pi = partition(low, high)
                if pi != -1:
                    quicksort(low, pi - 1)
                    quicksort(pi + 1, high)

        arr_copy = arr.copy()
        quicksort(0, len(arr_copy) - 1)
        return arr_copy

    def insertion_sort(self, arr):
        arr_copy = arr.copy()
        for i in range(1, len(arr_copy)):
            if self.stop_flag.is_set():
                break
            key = arr_copy[i]
            j = i - 1
            while j >= 0 and key < arr_copy[j]:
                arr_copy[j + 1] = arr_copy[j]
                j -= 1
            arr_copy[j + 1] = key
            self.update_progress("Insertion Sort", (i / len(arr_copy)) * 100)
            time.sleep(0.05)
        return arr_copy

    def radix_sort(self, arr):
        arr_copy = arr.copy()
        def counting_sort(exp1):
            if self.stop_flag.is_set():
                return
            n = len(arr_copy)
            output = [0] * n
            count = [0] * 10
            for i in range(n):
                index = arr_copy[i] // exp1
                count[index % 10] += 1
            for i in range(1, 10):
                count[i] += count[i - 1]
            i = n - 1
            while i >= 0:
                index = arr_copy[i] // exp1
                output[count[index % 10] - 1] = arr_copy[i]
                count[index % 10] -= 1
                i -= 1
            for i in range(n):
                arr_copy[i] = output[i]

        max1 = max(arr_copy)
        exp = 1
        iterations = 0
        while max1 / exp >= 1:
            if self.stop_flag.is_set():
                break
            counting_sort(exp)
            exp *= 10
            iterations += 1
            self.update_progress("Radix Sort", (iterations / 3) * 100)
            time.sleep(0.05)
        return arr_copy

    def bubble_sort(self, arr):
        arr_copy = arr.copy()
        n = len(arr_copy)
        for i in range(n):
            if self.stop_flag.is_set():
                break
            for j in range(0, n - i - 1):
                if arr_copy[j] > arr_copy[j + 1]:
                    arr_copy[j], arr_copy[j + 1] = arr_copy[j + 1], arr_copy[j]
            self.update_progress("Bubble Sort", (i / n) * 100)
            time.sleep(0.05)
        return arr_copy

    def selection_sort(self, arr):
        arr_copy = arr.copy()
        for i in range(len(arr_copy)):
            if self.stop_flag.is_set():
                break
            min_idx = i
            for j in range(i + 1, len(arr_copy)):
                if arr_copy[min_idx] > arr_copy[j]:
                    min_idx = j
            arr_copy[i], arr_copy[min_idx] = arr_copy[min_idx], arr_copy[i]
            self.update_progress("Selection Sort", (i / len(arr_copy)) * 100)
            time.sleep(0.05)
        return arr_copy

    def update_progress(self, algo, percent):
        if not self.stop_flag.is_set():
            self.progress_bars[algo].configure(value=percent)
            self.percent_labels[algo].configure(text=f"{percent:.1f}%")
            self.update()

    def sort_algorithm(self, algo_name, sort_func):
        self.progress_bars[algo_name].configure(value=0)
        self.percent_labels[algo_name].configure(text="0%")
        result = sort_func(self.array)
        if not self.stop_flag.is_set():
            self.progress_bars[algo_name].configure(value=100)
            self.percent_labels[algo_name].configure(text="Finished")
        else:
            self.percent_labels[algo_name].configure(text="Stopped")

    def start_sorting(self):
        if not self.sorting:
            self.sorting = True
            self.stop_flag.clear()
            sort_functions = {
                "Quick Sort": self.quick_sort,
                "Insertion Sort": self.insertion_sort,
                "Radix Sort": self.radix_sort,
                "Bubble Sort": self.bubble_sort,
                "Selection Sort": self.selection_sort
            }
            self.threads = {}
            for algo, func in sort_functions.items():
                thread = threading.Thread(target=self.sort_algorithm, args=(algo, func))
                self.threads[algo] = thread
                thread.start()

    def stop_sorting(self):
        if self.sorting:
            self.stop_flag.set()
            self.sorting = False

    def resume_sorting(self):
        if not self.sorting:
            self.start_sorting()

if __name__ == "__main__":
    app = SortingApp()
    app.mainloop()
```

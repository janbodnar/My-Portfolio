# Concurrency with sorting algorithms


## Threading in CLI

```python
import time
import random
import threading

# Initialize array globally
array = [random.randint(1, 1000) for _ in range(5000)]

def quick_sort(arr):
    print('quick sort started')
    start_time = time.time()

    def partition(arr, low, high):
        i = low - 1
        pivot = arr[high]
        for j in range(low, high):
            if arr[j] <= pivot:
                i += 1
                arr[i], arr[j] = arr[j], arr[i]
        arr[i + 1], arr[high] = arr[high], arr[i + 1]
        return i + 1

    def quicksort(arr, low, high):
        if low < high:
            pi = partition(arr, low, high)
            quicksort(arr, low, pi - 1)
            quicksort(arr, pi + 1, high)

    if not arr:
        return []
    if len(arr) == 1:
        return arr.copy()

    arr_copy = arr.copy()  # Fresh copy for this thread
    quicksort(arr_copy, 0, len(arr_copy) - 1)
    print('quick sort finished')
    end_time = time.time()
    return arr_copy, end_time - start_time

def insertion_sort(arr_copy):
    print('insertion sort started')
    start_time = time.time()
    # arr_copy = arr.copy()
    for i in range(1, len(arr_copy)):
        key = arr_copy[i]
        j = i - 1
        while j >= 0 and key < arr_copy[j]:
            arr_copy[j + 1] = arr_copy[j]
            j -= 1
        arr_copy[j + 1] = key

    print('insertion sort finished')

    end_time = time.time()
    runtime = end_time - start_time
    
    return arr_copy, runtime



def radix_sort(arr):
    print('radix sort started')
    start_time = time.time()

    def counting_sort(arr, exp1):
        n = len(arr)
        output = [0] * n
        count = [0] * 10
        for i in range(n):
            index = arr[i] // exp1
            count[index % 10] += 1
        for i in range(1, 10):
            count[i] += count[i - 1]
        i = n - 1
        while i >= 0:
            index = arr[i] // exp1
            output[count[index % 10] - 1] = arr[i]
            count[index % 10] -= 1
            i -= 1
        for i in range(n):
            arr[i] = output[i]

    arr_copy = arr.copy()  # Fresh copy for this thread
    max1 = max(arr_copy)
    exp = 1
    while max1 / exp >= 1:
        counting_sort(arr_copy, exp)
        exp *= 10

    print('radix sort finished')
    end_time = time.time()
    return arr_copy, end_time - start_time

def bubble_sort(arr_copy):

    print('bubble sort started')
    start_time = time.time()
    n = len(arr_copy)

    for i in range(n):
        for j in range(0, n - i - 1):
            if arr_copy[j] > arr_copy[j + 1]:
                arr_copy[j], arr_copy[j + 1] = arr_copy[j + 1], arr_copy[j]

    print('bubble sort finished')

    end_time = time.time()
    runtime = end_time - start_time
    
    return arr_copy, runtime

def selection_sort(arr_copy):

    print('Selection sort started')
    start_time = time.time()

    for i in range(len(arr_copy)):
        min_idx = i
        for j in range(i + 1, len(arr_copy)):
            if arr_copy[min_idx] > arr_copy[j]:
                min_idx = j
        arr_copy[i], arr_copy[min_idx] = arr_copy[min_idx], arr_copy[i]

    print('Selection sort finished')
    end_time = time.time()
    runtime = end_time - start_time
    
    return arr_copy, runtime

def sort_algorithm(algo_name, sort_func, input_array):
    result, runtime = sort_func(input_array.copy())  # Pass a fresh copy
    print(algo_name, result[:10], result[-10:], runtime)

def start_sorting():
    sort_functions = {
        "Insertion Sort": insertion_sort,
        "Radix Sort": radix_sort,
        "Bubble Sort": bubble_sort,
        "Quick Sort": quick_sort,
        "Selection Sort": selection_sort
    }
    
    threads = {}
    # Start all threads with a fresh copy of the array
    for algo, func in sort_functions.items():
        thread = threading.Thread(target=sort_algorithm, args=(algo, func, array))
        threads[algo] = thread
        thread.start()
    
    # Wait for all threads to finish
    for algo, thread in threads.items():
        thread.join()

def main():
    start_time = time.time()
    start_sorting()
    end_time = time.time()
    total_time = end_time - start_time
    print(f'total time: {total_time}')    

if __name__ == "__main__":
    main()
```

## Multiprocessing in CLI

```python
import time
import random
import multiprocessing as mp

# Initialize array globally
array = [random.randint(1, 1000) for _ in range(5000)]

# Sorting algorithms
def quick_sort(arr):
    print('quick sort started')
    start_time = time.time()

    def partition(arr, low, high):
        i = low - 1
        pivot = arr[high]
        for j in range(low, high):
            if arr[j] <= pivot:
                i += 1
                arr[i], arr[j] = arr[j], arr[i]
        arr[i + 1], arr[high] = arr[high], arr[i + 1]
        return i + 1

    def quicksort(arr, low, high):
        if low < high:
            pi = partition(arr, low, high)
            quicksort(arr, low, pi - 1)
            quicksort(arr, pi + 1, high)

    if not arr:
        return []
    if len(arr) == 1:
        return arr.copy()

    arr_copy = arr.copy()
    quicksort(arr_copy, 0, len(arr_copy) - 1)
    print('quick sort finished')
    end_time = time.time()
    return arr_copy, end_time - start_time

def insertion_sort(arr):
    print('insertion sort started')
    start_time = time.time()
    
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1
        while j >= 0 and key < arr[j]:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key

    print('insertion sort finished')
    end_time = time.time()
    return arr, end_time - start_time

def radix_sort(arr):
    print('radix sort started')
    start_time = time.time()

    def counting_sort(arr, exp1):
        n = len(arr)
        output = [0] * n
        count = [0] * 10
        for i in range(n):
            index = arr[i] // exp1
            count[index % 10] += 1
        for i in range(1, 10):
            count[i] += count[i - 1]
        i = n - 1
        while i >= 0:
            index = arr[i] // exp1
            output[count[index % 10] - 1] = arr[i]
            count[index % 10] -= 1
            i -= 1
        for i in range(n):
            arr[i] = output[i]

    arr_copy = arr.copy()
    max1 = max(arr_copy)
    exp = 1
    while max1 / exp >= 1:
        counting_sort(arr_copy, exp)
        exp *= 10

    print('radix sort finished')
    end_time = time.time()
    return arr_copy, end_time - start_time

def bubble_sort(arr):
    print('bubble sort started')
    start_time = time.time()
    
    n = len(arr)
    for i in range(n):
        for j in range(0, n - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]

    print('bubble sort finished')
    end_time = time.time()
    return arr, end_time - start_time

def selection_sort(arr):
    print('Selection sort started')
    start_time = time.time()

    for i in range(len(arr)):
        min_idx = i
        for j in range(i + 1, len(arr)):
            if arr[min_idx] > arr[j]:
                min_idx = j
        arr[i], arr[min_idx] = arr[min_idx], arr[i]

    print('Selection sort finished')
    end_time = time.time()
    return arr, end_time - start_time

def sort_algorithm(algo_name, sort_func, input_array, queue):
    result, runtime = sort_func(input_array.copy())
    # Put results in queue to return to main process
    queue.put((algo_name, result[:10], result[-10:], runtime))

def start_sorting():
    sort_functions = {
        "Insertion Sort": insertion_sort,
        "Radix Sort": radix_sort,
        "Bubble Sort": bubble_sort,
        "Quick Sort": quick_sort,
        "Selection Sort": selection_sort
    }
    
    processes = []
    queue = mp.Queue()  # Queue to collect results from processes
    
    # Start all processes
    for algo, func in sort_functions.items():
        process = mp.Process(target=sort_algorithm, args=(algo, func, array, queue))
        processes.append(process)
        process.start()
    
    # Wait for all processes to finish and collect results
    for process in processes:
        process.join()
    
    # Retrieve results from queue
    results = []
    for _ in range(len(sort_functions)):
        results.append(queue.get())
    
    # Print results
    for algo_name, first_10, last_10, runtime in results:
        print(algo_name, first_10, last_10, runtime)

def main():
    start_time = time.time()
    start_sorting()
    end_time = time.time()
    total_time = end_time - start_time
    print(f'total time: {total_time}')    

if __name__ == "__main__":
    main()
```



## Threading with UI using progress bars

Sorting is CPU-bound, so the overall so the sum of runtimes is not efficient.  
Also to show progress with progressbars, the tasks are artificially delayed, which distorts 
the true measurements. 

Lower resource footprint.  

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

## Multiprocessing with UI

The runtime of all tasks equals roughly to the runtime of the slowest one. It is truly parallel and more efficient.  
On the other hand, it brings complexity.  

Higher CPU and memory usage (visible in Task Manager).  

```python
import tkinter as tk
from tkinter import ttk
import time
import random
import multiprocessing as mp
from queue import Empty
import logging
import math

# Set up logging
logging.basicConfig(level=logging.DEBUG, format='%(processName)s: %(message)s')

# Standalone sorting algorithms
def quick_sort(arr, queue, stop_flag):
    logging.debug("Quick Sort started")
    def partition(low, high):
        if stop_flag.value:
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
        if stop_flag.value:
            return
        if low < high:
            pi = partition(low, high)
            if pi != -1:
                quicksort(low, pi - 1)
                quicksort(pi + 1, high)

    arr_copy = arr.copy()
    quicksort(0, len(arr_copy) - 1)
    if not stop_flag.value:
        queue.put(100)
        logging.debug("Quick Sort finished")
    time.sleep(0.1)  # Ensure queue flushes
    return arr_copy

def insertion_sort(arr, queue, stop_flag):
    logging.debug("Insertion Sort started")
    arr_copy = arr.copy()
    for i in range(1, len(arr_copy)):
        if stop_flag.value:
            break
        key = arr_copy[i]
        j = i - 1
        while j >= 0 and key < arr_copy[j]:
            arr_copy[j + 1] = arr_copy[j]
            j -= 1
        arr_copy[j + 1] = key
        queue.put((i / len(arr_copy)) * 100)
        time.sleep(0.1)
    if not stop_flag.value:
        queue.put(100)  # Final update
        logging.debug("Insertion Sort finished")
    time.sleep(0.1)  # Ensure queue flushes
    return arr_copy

def radix_sort(arr, queue, stop_flag):
    logging.debug("Radix Sort started")
    arr_copy = arr.copy()
    def counting_sort(exp1):
        if stop_flag.value:
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
    max_digits = math.ceil(math.log10(max1 + 1))  # Correct number of digits
    exp = 1
    iterations = 0
    while max1 / exp >= 1:
        if stop_flag.value:
            break
        counting_sort(exp)
        exp *= 10
        iterations += 1
        queue.put((iterations / max_digits) * 100)  # Correct progress
        time.sleep(0.1)
    if not stop_flag.value:
        queue.put(100)  # Final update
        logging.debug("Radix Sort finished")
    time.sleep(0.1)  # Ensure queue flushes
    return arr_copy

def bubble_sort(arr, queue, stop_flag):
    logging.debug("Bubble Sort started")
    arr_copy = arr.copy()
    n = len(arr_copy)
    for i in range(n):
        if stop_flag.value:
            break
        for j in range(0, n - i - 1):
            if arr_copy[j] > arr_copy[j + 1]:
                arr_copy[j], arr_copy[j + 1] = arr_copy[j + 1], arr_copy[j]
        queue.put((i / n) * 100)
        time.sleep(0.1)
    if not stop_flag.value:
        queue.put(100)  # Final update
        logging.debug("Bubble Sort finished")
    time.sleep(0.1)  # Ensure queue flushes
    return arr_copy

def selection_sort(arr, queue, stop_flag):
    logging.debug("Selection Sort started")
    arr_copy = arr.copy()
    for i in range(len(arr_copy)):
        if stop_flag.value:
            break
        min_idx = i
        for j in range(i + 1, len(arr_copy)):
            if arr_copy[min_idx] > arr_copy[j]:
                min_idx = j
        arr_copy[i], arr_copy[min_idx] = arr_copy[min_idx], arr_copy[i]
        queue.put((i / len(arr_copy)) * 100)
        time.sleep(0.1)
    if not stop_flag.value:
        queue.put(100)  # Final update
        logging.debug("Selection Sort finished")
    time.sleep(0.1)  # Ensure queue flushes
    return arr_copy

def sort_algorithm(algo_name, sort_func, arr, queue, stop_flag):
    logging.debug(f"Starting {algo_name}")
    sort_func(arr, queue, stop_flag)
    logging.debug(f"{algo_name} completed")

class SortingApp(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Sorting Algorithms")
        self.geometry("800x600")
        
        # Set dark theme
        self.configure(bg='#222222')
        style = ttk.Style()
        style.theme_use('clam')
        style.configure('TFrame', background='#222222', font=('Helvetica', 14))
        style.configure('TLabel', background='#222222', foreground='#ffffff', font=('Helvetica', 14))
        style.configure('TButton', background='#404040', foreground='#ffffff')
        style.map('TButton', background=[('active', '#606060')])
        style.configure('TProgressbar', background='#00cc00', troughcolor='#404040')
        
        # Initialize variables
        self.array = [random.randint(1, 100) for _ in range(50)]
        self.sorting = False
        self.manager = mp.Manager()
        self.stop_flag = self.manager.Value('b', False)
        self.progress_queues = {algo: mp.Queue() for algo in 
                              ["Quick Sort", "Insertion Sort", "Radix Sort", 
                               "Bubble Sort", "Selection Sort"]}
        
        # UI Elements
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
        
        self.start_button = ttk.Button(button_frame, text="Start", command=self.start_sorting)
        self.start_button.pack(side='left', padx=5)
        
        self.stop_button = ttk.Button(button_frame, text="Stop", command=self.stop_sorting)
        self.stop_button.pack(side='left', padx=5)
        
        self.resume_button = ttk.Button(button_frame, text="Resume", command=self.resume_sorting)
        self.resume_button.pack(side='left', padx=5)
        
        self.processes = {}
        
        # Start progress update loop
        self.after(100, self.update_progress_from_queues)

    def update_progress_from_queues(self):
        updated = False
        for algo, queue in self.progress_queues.items():
            try:
                while True:
                    percent = queue.get_nowait()
                    logging.debug(f"Received {percent}% for {algo}")
                    self.progress_bars[algo].configure(value=percent)
                    if percent >= 100 and not self.stop_flag.value:
                        self.percent_labels[algo].configure(text="Finished")
                    else:
                        self.percent_labels[algo].configure(text=f"{percent:.1f}%")
                    self.update()
                    updated = True
            except Empty:
                pass
        
        if updated or self.sorting or any(not q.empty() for q in self.progress_queues.values()):
            self.after(50, self.update_progress_from_queues)
        else:
            logging.debug("No updates received")

    def start_sorting(self):
        if not self.sorting:
            logging.debug("Starting sorting processes")
            self.sorting = True
            self.stop_flag.value = False
            sort_functions = {
                "Quick Sort": quick_sort,
                "Insertion Sort": insertion_sort,
                "Radix Sort": radix_sort,
                "Bubble Sort": bubble_sort,
                "Selection Sort": selection_sort
            }
            self.processes = {}
            for algo, func in sort_functions.items():
                while not self.progress_queues[algo].empty():
                    try:
                        self.progress_queues[algo].get_nowait()
                    except Empty:
                        break
                self.progress_bars[algo].configure(value=0)
                self.percent_labels[algo].configure(text="0%")
                process = mp.Process(target=sort_algorithm,
                                   args=(algo, func, self.array, 
                                         self.progress_queues[algo], self.stop_flag))
                self.processes[algo] = process
                process.start()
                logging.debug(f"Started process for {algo}")
            time.sleep(0.1)
            self.update_progress_from_queues()

    def stop_sorting(self):
        if self.sorting:
            logging.debug("Stopping sorting processes")
            self.stop_flag.value = True
            for algo, proc in self.processes.items():
                if proc.is_alive():
                    proc.terminate()
                    logging.debug(f"Terminated {algo}")
            self.sorting = False
            for algo in self.processes:
                if self.progress_bars[algo].cget('value') < 100:
                    self.percent_labels[algo].configure(text="Stopped")

    def resume_sorting(self):
        if not self.sorting:
            self.start_sorting()

if __name__ == "__main__":
    mp.freeze_support()
    app = SortingApp()
    app.mainloop()
```




import tkinter as tk
from tkinter import messagebox

class TaskManagerApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Lista de Tareas")
        self.root.geometry("400x500")
        self.root.configure(bg="#f0f0f0")

        # Campo de entrada
        self.task_entry = tk.Entry(root, font=("Arial", 14))
        self.task_entry.pack(pady=10, padx=10, fill=tk.X)
        self.task_entry.focus()

        # Botones
        self.add_btn = tk.Button(root, text="Añadir Tarea", command=self.add_task, bg="#4CAF50", fg="white")
        self.add_btn.pack(pady=5)

        self.complete_btn = tk.Button(root, text="Marcar como Completada", command=self.complete_task, bg="#2196F3", fg="white")
        self.complete_btn.pack(pady=5)

        self.delete_btn = tk.Button(root, text="Eliminar Tarea", command=self.delete_task, bg="#f44336", fg="white")
        self.delete_btn.pack(pady=5)

        # Lista de tareas
        self.task_listbox = tk.Listbox(root, font=("Arial", 12), selectmode=tk.SINGLE)
        self.task_listbox.pack(pady=10, padx=10, expand=True, fill=tk.BOTH)

        # Atajos de teclado
        root.bind('<Return>', lambda event: self.add_task())
        root.bind('<c>', lambda event: self.complete_task())
        root.bind('<d>', lambda event: self.delete_task())
        root.bind('<Delete>', lambda event: self.delete_task())
        root.bind('<Escape>', lambda event: root.destroy())

    def add_task(self):
        task = self.task_entry.get().strip()
        if task:
            self.task_listbox.insert(tk.END, task)
            self.task_entry.delete(0, tk.END)
        else:
            messagebox.showwarning("Campo vacío", "Por favor, escribe una tarea.")

    def complete_task(self):
        selected = self.task_listbox.curselection()
        if selected:
            idx = selected[0]
            task_text = self.task_listbox.get(idx)
            if not task_text.startswith("[✔] "):
                self.task_listbox.delete(idx)
                self.task_listbox.insert(idx, "[✔] " + task_text)
        else:
            messagebox.showinfo("Seleccionar Tarea", "Selecciona una tarea para marcarla como completada.")

    def delete_task(self):
        selected = self.task_listbox.curselection()
        if selected:
            self.task_listbox.delete(selected[0])
        else:
            messagebox.showinfo("Seleccionar Tarea", "Selecciona una tarea para eliminarla.")

if __name__ == "__main__":
    root = tk.Tk()
    app = TaskManagerApp(root)
    root.mainloop()

import tkinter as tk
from tkinter import messagebox

def beregn_medicin():
    try:
        # Hent værdier fra inputfelter
        alder = int(alder_entry.get())
        vægt = float(vægt_entry.get())
        køn = køn_var.get()
        dosis = float(dosis_entry.get())
        styrke = float(styrke_entry.get())
        dosis_per_kg = float(dosis_per_kg_entry.get())
        
        # Beregn vægtbaseret dosis
        samlet_dosis = dosis_per_kg * vægt

        # Justering baseret på alder
        if alder < 12:
            samlet_dosis *= 0.75  # Juster dosis for børn under 12
        elif alder > 65:
            samlet_dosis *= 0.85  # Juster dosis for ældre over 65

        # Justering baseret på køn
        if køn == 'kvinde':
            samlet_dosis *= 0.9  # Juster dosis for kvinder
        
        # Beregn mængde medicin
        mængde_vægtbaseret = samlet_dosis / styrke
        mængde_fastsat = dosis / styrke
        
        # Vis resultater
        result_label.config(text=f"Vægtbaseret dosis: {mængde_vægtbaseret:.2f} ml\nFast dosis: {mængde_fastsat:.2f} ml")

    except ValueError:
        # Fejlhåndtering for ugyldige input
        messagebox.showerror("Fejl", "Ugyldigt input! Sørg for at indtaste korrekte numeriske værdier.")

def nulstil_felter():
    # Nulstil alle inputfelter
    alder_entry.delete(0, tk.END)
    vægt_entry.delete(0, tk.END)
    dosis_entry.delete(0, tk.END)
    styrke_entry.delete(0, tk.END)
    dosis_per_kg_entry.delete(0, tk.END)
    result_label.config(text="")

# Opret hovedvinduet
root = tk.Tk()
root.title("Medicinberegningsprogram")

# Opret inputfelter og labels
tk.Label(root, text="Alder (år):").grid(row=0, column=0, padx=5, pady=5)
alder_entry = tk.Entry(root)
alder_entry.grid(row=0, column=1, padx=5, pady=5)

tk.Label(root, text="Vægt (kg):").grid(row=1, column=0, padx=5, pady=5)
vægt_entry = tk.Entry(root)
vægt_entry.grid(row=1, column=1, padx=5, pady=5)

tk.Label(root, text="Køn:").grid(row=2, column=0, padx=5, pady=5)
køn_var = tk.StringVar(value="mand")
tk.Radiobutton(root, text="Mand", variable=køn_var, value="mand").grid(row=2, column=1)
tk.Radiobutton(root, text="Kvinde", variable=køn_var, value="kvinde").grid(row=2, column=2)

tk.Label(root, text="Ønsket dosis (mg):").grid(row=3, column=0, padx=5, pady=5)
dosis_entry = tk.Entry(root)
dosis_entry.grid(row=3, column=1, padx=5, pady=5)

tk.Label(root, text="Styrke af medicin (mg/ml):").grid(row=4, column=0, padx=5, pady=5)
styrke_entry = tk.Entry(root)
styrke_entry.grid(row=4, column=1, padx=5, pady=5)

tk.Label(root, text="Dosis per kg (mg/kg):").grid(row=5, column=0, padx=5, pady=5)
dosis_per_kg_entry = tk.Entry(root)
dosis_per_kg_entry.grid(row=5, column=1, padx=5, pady=5)

# Opret beregningsknap og nulstilknap
beregn_button = tk.Button(root, text="Beregn dosis", command=beregn_medicin)
beregn_button.grid(row=6, column=0, columnspan=2, pady=10)

nulstil_button = tk.Button(root, text="Nulstil", command=nulstil_felter)
nulstil_button.grid(row=6, column=2, pady=10)

# Opret resultatlabel
result_label = tk.Label(root, text="", fg="blue")
result_label.grid(row=7, column=0, columnspan=3, pady=10)

# Kør GUI
root.mainloop()

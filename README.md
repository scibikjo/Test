def taschenrechner():
    print("=== Einfacher Taschenrechner ===")
    print("Verfügbare Operationen:")
    print("  + : Addition")
    print("  - : Subtraktion")
    print("  * : Multiplikation")
    print("  / : Division")
    print("  ^ : Potenzieren")
    
    while True:
        # Operator abfragen
        operator = input("\nWähle einen Operator (+, -, *, /, ^) oder 'q' zum Beenden: ").strip()
        
        if operator.lower() == 'q':
            print("Taschenrechner wird beendet. Auf Wiedersehen!")
            break
            
        if operator not in ['+', '-', '*', '/', '^']:
            print("Ungültiger Operator! Bitte erneut versuchen.")
            continue
            
        # Zahlen eingeben
        try:
            zahl1 = float(input("Erste Zahl eingeben: "))
            zahl2 = float(input("Zweite Zahl eingeben: "))
        except ValueError:
            print("Fehler: Bitte gib eine gültige Zahl ein.")
            continue
            
        # Berechnung durchführen
        if operator == '+':
            ergebnis = zahl1 + zahl2
        elif operator == '-':
            ergebnis = zahl1 - zahl2
        elif operator == '*':
            ergebnis = zahl1 * zahl2
        elif operator == '/':
            if zahl2 == 0:
                print("Fehler: Division durch Null ist nicht erlaubt!")
                continue
            ergebnis = zahl1 / zahl2
        elif operator == '^':
            ergebnis = zahl1 ** zahl2
            
        # Ergebnis ausgeben (Formatiert als Ganzzahl, wenn keine Nachkommastellen vorhanden sind)
        if ergebnis.is_integer():
            ergebnis = int(ergebnis)
            
        print(f"Ergebnis: {zahl1} {operator} {zahl2} = {ergebnis}")

# Programm starten
if __name__ == "__main__":
    taschenrechner()

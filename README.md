# project-nidan-digital-twin
Micro-Vascular Retinal Digital Twin for Diabetic Nephropathy Prediction
# Sample implementation structure for nidaan_twin_engine.py

import numpy as np
import pandas as pd

def calculate_vascular_risk(microaneurysms: int, vessel_density: float, avr: float) -> float:
    """Calculates structural micro-vascular degradation score (0.0 to 1.0)."""
    ma_score = min(microaneurysms / 20.0, 1.0)
    density_score = max(0.0, (0.45 - vessel_density) / 0.20)
    avr_score = max(0.0, (0.67 - avr) / 0.20)
    return 0.4 * ma_score + 0.35 * density_score + 0.25 * avr_score

def project_egfr_decline(baseline_egfr: float, hba1c: float, vascular_risk: float, rhr_elevation: float) -> float:
    """Projects estimated eGFR at 6 months based on dynamic stress integration."""
    base_drop_rate = (hba1c - 6.0) * 0.8 if hba1c > 6.0 else 0.1
    vascular_factor = 1.0 + (vascular_risk * 2.5)
    autonomic_factor = 1.0 + max(0.0, (rhr_elevation - 5.0) * 0.1)
    
    six_month_decline = base_drop_rate * vascular_factor * autonomic_factor
    return max(10.0, baseline_egfr - six_month_decline)

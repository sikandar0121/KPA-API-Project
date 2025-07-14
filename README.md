# KPA-API-Project

from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
from typing import List, Optional

app = FastAPI()

class KPA(BaseModel):
    id: int
    name: str
    description: Optional[str] = None
    score: Optional[float] = None

# Fake database
kpas_db: List[KPA] = []

@app.post("/kpas", response_model=KPA)
def create_kpa(kpa: KPA):
    kpas_db.append(kpa)
    return kpa

@app.get("/kpas", response_model=List[KPA])
def list_kpas():
    return kpas_db

@app.get("/kpas/{kpa_id}", response_model=KPA)
def get_kpa(kpa_id: int):
    for kpa in kpas_db:
        if kpa.id == kpa_id:
            return kpa
    raise HTTPException(status_code=404, detail="KPA not found")

@app.put("/kpas/{kpa_id}", response_model=KPA)
def update_kpa(kpa_id: int, updated_kpa: KPA):
    for i, kpa in enumerate(kpas_db):
        if kpa.id == kpa_id:
            kpas_db[i] = updated_kpa
            return updated_kpa
    raise HTTPException(status_code=404, detail="KPA not found")

@app.delete("/kpas/{kpa_id}")
def delete_kpa(kpa_id: int):
    for i, kpa in enumerate(kpas_db):
        if kpa.id == kpa_id:
            del kpas_db[i]
            return {"message": "KPA deleted"}
    raise HTTPException(status_code=404, detail="KPA not found")

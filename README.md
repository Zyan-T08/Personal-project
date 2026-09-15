def test_stored_incident_status_can_be_updated(
    tmp_path,
    monkeypatch
):
    test_database = tmp_path / "test_incidents.db"

    monkeypatch.setattr(
        "app.services.database.DATABASE_PATH",
        test_database
    )

    initialize_database()

    incident = {
        "type": "brute_force",
        "severity": "high",
        "username": "admin",
        "ip_address": "203.0.113.50",
        "risk_score": 75,
        "status": "new",
        "description": "Test incident.",
        "created_at": "2026-09-14T10:20:00"
    }

    incident_id = save_incident(incident)

    updated = update_incident_status(
        incident_id,
        "investigating"
    )

    assert updated is not None
    assert updated["id"] == incident_id
    assert updated["status"] == "investigating"

    import pytest

from app.services.database import (
    initialize_database,
    save_incident,
    update_incident_status
)

from app.services.incident_service import (
    add_incident_metadata,
    update_incident_status as update_in_memory_status
)

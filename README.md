# True-1
from typing import List, Dict, Set, Any

def generate_student_report(students: List[Dict[str, Any]]) -> Dict[str, Any]:
    if not students:
        raise ValueError("Talabalar ro'yxati bo'sh bo'lishi mumkin emas!")

    all_names: List[str] = [s["ism"] for s in students]

    grant_students: List[str] = [s["ism"] for s in students if s.get("grant", False)]

    contract_status: List[str] = [
        f"{s['ism']} - Stipendiyali" if s["baho"] >= 80 else f"{s['ism']} - Stipendiyasiz"
        for s in students if not s.get("grant", False)
    ]

    all_grades: List[int] = [s["baho"] for s in students]
    average_grade: float = sum(all_grades) / len(all_grades)

    student_degrees: Dict[str, str] = {
        s["ism"]: "A" if s["baho"] >= 85 else ("B" if s["baho"] >= 70 else "C")
        for s in students
    }

    unique_tags: Set[str] = {tag for s in students for tag in s.get("teglar", [])}

    all_semester_grades: List[int] = [
        grade
        for s in students
        for grade in s.get("semestr_baholari", [])
    ]

    return {
        "barcha_talabalar": all_names,
        "grant_talabalar": grant_students,
        "kontrakt_holati": contract_status,
        "ortacha_baho": round(average_grade, 2),
        "talaba_darajalari": student_degrees,
        "takrorlanmas_teglar": unique_tags,
        "barcha_semestr_baholari": all_semester_grades
    }


if __name__ == "__main__":
    mock_students = [
        {
            "ism": "Sunnat", "baho": 90, "grant": True,
            "teglar": ["math", "python"], "semestr_baholari": [88, 92]
        },
        {
            "ism": "Jasur", "baho": 75, "grant": False,
            "teglar": ["python", "ai"], "semestr_baholari": [70, 80]
        },
        {
            "ism": "Kamron", "baho": 65, "grant": False,
            "teglar": ["math", "web"], "semestr_baholari": [60, 70]
        },
        {
            "ism": "Gulbotir", "baho": 85, "grant": True,
            "teglar": ["ai", "data"], "semestr_baholari": [80, 90]
        }
    ]

    try:
        report = generate_student_report(mock_students)

        print("📊 TALABALAR HISOBOTI:\n" + "=" * 30)
        print(f"• Ismlar: {report['barcha_talabalar']}")
        print(f"• Grantdagilar: {report['grant_talabalar']}")
        print(f"• Kontraktlar holati: {report['kontrakt_holati']}")
        print(f"• O'rtacha baho: {report['ortacha_baho']}")
        print(f"• Darajalar (Dict): {report['talaba_darajalari']}")
        print(f"• Teglar (Set): {report['takrorlanmas_teglar']}")
        print(f"• Flatten baholar (Nested): {report['barcha_semestr_baholari']}")

    except ValueError as e:
        print(f"Xatolik: {e}")

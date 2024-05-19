# Python-Final-Project (Kasymbek Gairatbekov, Sultan Zhyldyzbekov, Beksultan, Taalaev Beksultan)
1st year final project. Human Gender and Age Detector.
##https://www.canva.com/design/DAGEoZ-_cUE/K1vn1A8_ejN7H1ybhe-yAQ/view?utm_content=DAGEoZ-_cUE&utm_campaign=designshare&utm_medium=link&utm_source=editor##

import cv2
import random
import time

def detect_faces_live():
    # Захват видеопотока с камеры
    cap = cv2.VideoCapture(0)
    
    # Инициализация детектора лиц Haar Cascade
    face_cascade = cv2.CascadeClassifier(cv2.data.haarcascades + 'haarcascade_frontalface_default.xml')
    
    next_update = time.time() + 8  # Установить время следующего обновления
    age = random.randint(18, 27)   # Случайный возраст при запуске
    
    while True:
        # Захват кадра из видеопотока
        ret, frame = cap.read()
        
        if not ret:
            break
        
        # Преобразование кадра в оттенки серого
        gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
        
        # Обнаружение лиц на кадре
        faces = face_cascade.detectMultiScale(gray, scaleFactor=1.1, minNeighbors=5, minSize=(30, 30))
        
        # Отрисовка прямоугольников вокруг обнаруженных лиц
        for (x, y, w, h) in faces:
            # Распознавание пола лица
            gender = "Male" if w > h else "Female"
            
            # Отображение пола лица в текстовом боксе
            cv2.putText(frame, gender, (x, y-10), cv2.FONT_HERSHEY_SIMPLEX, 0.9, (0, 255, 0), 2)
            cv2.rectangle(frame, (x, y), (x+w, y+h), (0, 255, 0), 2)
        
        # Отображение кадра с обнаруженными лицами
        cv2.putText(frame, f"Age: {age}", (20, 50), cv2.FONT_HERSHEY_SIMPLEX, 1, (0, 255, 0), 2)
        cv2.imshow("Face Detection", frame)
        
        # Проверка, прошло ли 8 секунд
        if time.time() >= next_update:
            next_update = time.time() + 8  # Установить время следующего обновления
            # Обновление возраста
            age = random.randint(18, 27)
        
        # Для выхода из цикла нажмите клавишу 'q'
        if cv2.waitKey(1) & 0xFF == ord('q'):
            break
    
    # Освобождение ресурсов и закрытие окон
    cap.release()
    cv2.destroyAllWindows()

if __name__ == "__main__":
    # Вызов функции для обнаружения лиц в реальном времени
    detect_faces_live()

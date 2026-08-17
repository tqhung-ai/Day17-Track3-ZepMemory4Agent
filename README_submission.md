# Short-term memory và compaction

Trong thử nghiệm E10, compaction giữ constraint `REVIEW-DEADLINE-1600`: project review diễn ra vào Friday lúc `16:00`; thông tin này được lưu trong `<DURABLE_NOTES>` nên vẫn còn dù các raw turn cũ bị loại khỏi cửa sổ gần nhất. Chiến lược `buffer` giữ toàn bộ transcript và không compaction, vì vậy hội thoại dài làm số token tăng tuyến tính (token explosion) và có thể tràn context window. `Sliding` (mặc định) kết hợp summary, durable notes và các lượt gần nhất để giữ state quan trọng với chi phí token bị giới hạn.

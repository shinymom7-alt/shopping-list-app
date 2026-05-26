# Created: 2026-05-26 11:32
# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 실행 방법

별도의 빌드 과정 없음. `shopping-list.html`을 브라우저에서 직접 열면 동작한다.

Playwright MCP로 자동 테스트할 때는 파일의 절대 경로를 브라우저에서 열어 진행한다.

## 아키텍처

단일 HTML 파일(`shopping-list.html`) 안에 CSS와 JavaScript가 모두 포함된 구조다.

**데이터 흐름:**
- `items` 배열이 유일한 상태 (메모리 + `localStorage` 동기화)
- 모든 변경은 `save()` → `render()` 순서로 끝남
- `render()`는 항상 전체 목록을 다시 그림 (DOM diffing 없음)

**주요 함수:**
- `addItem()` — 새 항목을 배열 앞에 삽입(`unshift`)
- `toggle(idx)` — 완료 상태 반전
- `remove(idx)` — 인덱스로 항목 제거
- `clearDone()` — 완료된 항목 일괄 제거
- `escHtml(str)` — XSS 방지용 HTML 이스케이프 (innerHTML에 사용자 입력을 넣기 전에 반드시 거침)

**localStorage 키:** `shopping` (JSON 배열)

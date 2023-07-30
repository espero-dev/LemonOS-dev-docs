# Basic window drawing
```c++
#include <Lemon/GUI/Window.h>
#include <Lemon/Graphics/Graphics.h>
#include <Lemon/Core/Keyboard.h>

#include <map>
#include <list>
#include <unistd.h>

#define WIDTH 24
#define HEIGHT 16

Lemon::GUI::Window* win;

//////////////////////////
// Button
//////////////////////////
Button::Button(const char* _label, rect_t _bounds) : Widget(_bounds) {
    label = _label;
    labelLength = Graphics::GetTextLength(label.c_str());
}

void Button::SetLabel(const char* _label) {
    label = _label;
    labelLength = Graphics::GetTextLength(label.c_str());
}

void Button::DrawButtonLabel(surface_t* surface) {
    rgba_colour_t colour;
    vector2i_t btnPos = fixedBounds.pos;

    colour = Theme::Current().ColourTextLight();

    if (labelAlignment == TextAlignment::Centre) {
        Graphics::DrawString(label.c_str(), btnPos.x + (fixedBounds.size.x / 2) - (labelLength / 2),
                             btnPos.y + (fixedBounds.size.y / 2 - 6 - 2), colour.r, colour.g, colour.b, surface);
    } else {
        Graphics::DrawString(label.c_str(), btnPos.x + 3, btnPos.y + (fixedBounds.size.y / 2 - 6 - 2), colour.r,
                             colour.g, colour.b, surface);
    }
}

void Button::Paint(surface_t* surface) {
    if (pressed) {
        Lemon::Graphics::DrawRoundedRect(fixedBounds, Theme::Current().ColourBorder(), 5, 5, 5, 5, surface);
    } else {
        Rect innerRect = {fixedBounds.pos + Vector2i{1, 1}, fixedBounds.size - Vector2i{2, 3}};
        ;
        Lemon::Graphics::DrawRoundedRect(fixedBounds, Theme::Current().ColourBorder(), 5, 5, 5, 5, surface);
        if (fixedBounds.Contains(window->lastMousePos)) {
            Lemon::Graphics::DrawRoundedRect(innerRect, Theme::Current().ColourForeground(), 5, 5, 5, 5, surface);
        } else {
            Lemon::Graphics::DrawRoundedRect(innerRect, Theme::Current().ColourButton(), 5, 5, 5, 5, surface);
        }
    }

    DrawButtonLabel(surface);
}

void Button::OnMouseDown(__attribute__((unused)) vector2i_t mousePos) { pressed = true; }

void Button::OnMouseUp(vector2i_t mousePos) {
    if (Graphics::PointInRect(fixedBounds, mousePos) && e.onPress.handler)
        e.onPress();

    pressed = false;
}

/*void DrawButton(unsigned int x, unsigned int y, unsigned int width, unsigned int height, uint8_t r, uint8_t g, uint8_t b, ) {
    uint32_t colour = r << 16 | g << 8 | b;

    for (unsigned i = 0; i < height; i++) {
        for (unsigned j = 0; j < width; j++) {
            ((uint32_t*)videoMemory)[((i + y) * screenWidth) + j + x] = colour;
        }
    }
}*/

int main(){
	win = new Lemon::GUI::Window(base64_decode("RlBT"), { SNAKE_CELL_SIZE * SNAKE_MAP_WIDTH, SNAKE_CELL_SIZE * SNAKE_MAP_HEIGHT });
	win->OnPaint = OnPaint;

  Lemon::GUI::Bitmap* drawBox = new Lemon::GUI::Bitmap({win->GetSize().x/2, win->GetSize().y/2, 25, 45}); // Rectangle clip test
  win->AddWidget(drawBox);
  rect_t r1 = {10, 20, 300, 70};
  rect_t r2 = {50, 10, 100, 60};
  auto rClips = r1.Split(r2);
  for(auto rect : rClips){
      Lemon::Graphics::DrawRect(rect, (rgba_colour_t){83, 0, 0, 142}, &drawBox->surface);
      Lemon::Graphics::DrawString("lil menu", drawbox.x, drawbox.y, 0, 0, 0, &drawBox->surface);
      Lemon::Graphics::DrawString("im watching", drawbox.x, drawbox.y - 10, 0, 0, 0, &drawBox->surface);
      Lemon::Graphics::DrawString("you :)", drawbox.x, drawbox.y - 20, 0, 0, 0, &drawBox->surface);
  }
  Lemon::Graphics::DrawRect(r2, {0, 255, 255, 255}, &drawBox->surface);

  Lemon::GUI::Bitmap* mod = new Lemon::GUI::Bitmap({60, 60, 40, 40}); // Rectangle clip test
  win->AddWidget(mod);
  rect_t rt = {10, 20, 300, 70};
  rect_t rp = {50, 10, 100, 60};
  auto rClip = rt.Split(rp);
  for(auto rect : rClip){
      Lemon::Graphics::DrawRect(rect, (rgba_colour_t){83, 0, 0, 142}, &drawBox->surface);
      Lemon::Button::Button* blue = new Lemon::Button::Button({60, 60, 40, 40});
  }
  Lemon::Graphics::DrawRect(rp, {0, 255, 255, 255}, &drawBox->surface);

	srand(time(nullptr));

	Reset();
	return 0;
}
```

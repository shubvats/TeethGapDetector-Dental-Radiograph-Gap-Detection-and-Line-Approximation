close all;
% Load the image
image = imread('teeth_sample.png');
% Check if the image is already in grayscale
if size(image, 3) == 3
% Convert the image to grayscale
gray_image = rgb2gray(image);
else
gray_image = image; % Image is already grayscale
end
% Enhance contrast using adapthisteq
enhanced_image = adapthisteq(gray_image);
% Divide the enhanced image into 6 vertical parts
num_stripes = 6;
[~, width] = size(enhanced_image);
stripe_width = floor(width / num_stripes);
% Initialize arrays to store midpoints of dark regions
midpoints_x = [];
midpoints_y = [];
% Process each stripe separately
figure;
for i = 1:num_stripes
% Extract the stripe
start_col = (i - 1) * stripe_width + 1;
end_col = min(i * stripe_width, width);
stripe = enhanced_image(:, start_col:end_col);

% Threshold the stripe to detect dark regions
threshold = graythresh(stripe);
binary_stripe = imbinarize(stripe, threshold);

% Find horizontal dark regions
dark_regions = sum(binary_stripe, 2) == 0;

% Find midpoint of dark region
dark_indices = find(dark_regions);
if ~isempty(dark_indices)
midpoint_index = dark_indices(round(length(dark_indices)/2));
midpoints_x = [midpoints_x, (start_col + end_col) / 2];
midpoints_y = [midpoints_y, midpoint_index];
end

% Display the stripe with detected dark regions
subplot(1, num_stripes, i);
imshow(stripe);
hold on;
plot([1, size(stripe, 2)], [midpoint_index, midpoint_index], 'r', 'LineWidth',
2);
title(['Stripe ', num2str(i)]);
hold off;
end
% Add endpoints of the first and last horizontal lines
if ~isempty(midpoints_x)
first_endpoint = [1, midpoints_y(1)];
last_endpoint = [width, midpoints_y(end)];
midpoints_x = [first_endpoint(1), midpoints_x, last_endpoint(1)];
midpoints_y = [first_endpoint(2), midpoints_y, last_endpoint(2)];
% Fit quadratic curve through midpoints
p = polyfit(midpoints_x, midpoints_y, 2);
curve_x = linspace(min(midpoints_x), max(midpoints_x), 100);
curve_y = polyval(p, curve_x);
% Display the original image with the curve overlaid
figure;
imshow(image);
hold on;
plot(curve_x, curve_y, 'b', 'LineWidth', 2);
title('Quadratic Curve along Gap on Original Image');

% Parameters for perpendicular lines
upper_jaw_step_size = 20; % Change as needed
lower_jaw_step_size = 15; % Change as needed
line_length = 150; % Change as needed
intensity_threshold = 19000;
intensity_threshold1 = 20000;
% Loop for upper jaw
for i = 1:6:numel(curve_x)
% Get coordinates of current point on curve
x = curve_x(i);
y = curve_y(i);

% Compute slope of curve at that point
slope_curve = polyval(polyder(p), x);

% Compute slope of perpendicular line
slope_perp = -1 / slope_curve;

% Compute y-intercept of perpendicular line
y_intercept = y - slope_perp * x;

% Compute change in x and y for line length
delta_x = sqrt(line_length^2 / (1 + slope_perp^2));
delta_y = slope_perp * delta_x;

% Compute start and end points for the line
start_point = [x - delta_x, y - delta_y];
end_point = [x + delta_x, y + delta_y];

% Ensure upper line stays only above the curve
y_start = start_point(2);
y_end = end_point(2);
x_start = start_point(1);
x_end = end_point(1);

if y_start > y
y_start = y;
x_start = (y_start - y_intercept) / slope_perp;
end

if y_end > y
y_end = y;
x_end = (y_end - y_intercept) / slope_perp;
end

% Profile intensity along the line
[line_x, line_y, profile] = improfile(enhanced_image, [x_start, x_end],
[y_start, y_end]);

% Compute sum of intensities
intensity_sum = sum(profile);

% Plot the line only if intensity sum is less than threshold
if intensity_sum < intensity_threshold1
plot([x_start, x_end], [y_start, y_end], 'r', 'LineWidth', 1);
end
end

% Loop for lower jaw
for i = 1:7:numel(curve_x)
% Get coordinates of current point on curve
x = curve_x(i);
y = curve_y(i);

% Compute slope of curve at that point
slope_curve = polyval(polyder(p), x);

% Compute slope of perpendicular line
slope_perp = -1 / slope_curve;

% Compute y-intercept of perpendicular line
y_intercept = y - slope_perp * x;

% Compute change in x and y for line length
delta_x = sqrt(line_length^2 / (1 + slope_perp^2));
delta_y = slope_perp * delta_x;

% Compute start and end points for the line
start_point = [x - delta_x, y - delta_y];
end_point = [x + delta_x, y + delta_y];

% Ensure lower line stays only below the curve
y_start = start_point(2);
y_end = end_point(2);
x_start = start_point(1);
x_end = end_point(1);

if y_start < y
y_start = y;
x_start = (y_start - y_intercept) / slope_perp;
end

if y_end < y
y_end = y;
x_end = (y_end - y_intercept) / slope_perp;
end

% Profile intensity along the line
[line_x, line_y, profile] = improfile(enhanced_image, [x_start, x_end],
[y_start, y_end]);

% Compute sum of intensities
intensity_sum = sum(profile);

% Plot the line only if intensity sum is less than threshold
if intensity_sum < intensity_threshold
plot([x_start, x_end], [y_start, y_end], 'g', 'LineWidth', 1);
end
end
hold off;
else
disp('No dark regions detected.');
end
